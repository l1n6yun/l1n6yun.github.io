title: thinkphp 后台地址加密
tags:
  - thinkphp
date: 2019-11-20 10:25:59
categories:
---

`route.php` 文件

```php
return array (
  'l1n6yun$' => 'admin/Index/index',
);
```

``app\admin\controller\AdminBaseController` 类

```php
protected function initialize()
{
    parent::initialize();

    // 获取登陆session
    $sessionAdminId = session('ADMIN_ID');

    // 如果没有登陆跳转到登陆页面
    if (empty($sessionAdminId)) {
        return $this->redirect(url("admin/Public/login"));
    }
}
```

`app\admin\controller\IndexController` 类

```php
public function initialize()
{
    // 后台设置
    $adminSettings = cmf_get_option('admin_settings');
    
    // $adminSettings['admin_password'] = "l1n6yun" 后台加密地址 
    if (empty($adminSettings['admin_password']) || $this->request->path() == $adminSettings['admin_password']) {
        $adminId = cmf_get_current_admin_id();
        if (empty($adminId)) {
            session("__LOGIN_BY_CMF_ADMIN_PW__", 1);//设置后台登录加密码
        }
    }

    parent::initialize();
}
```

`app\admin\controller\PublicController` 类

```php
class PublicController extends AdminBaseController
{
	public function initialize()
    {
    }
    
    public function login()
    {
    	// 设置后台登录加密码
        $loginAllowed = session("__LOGIN_BY_CMF_ADMIN_PW__");
        if (empty($loginAllowed)) {
            return redirect(cmf_get_root() . "/");
		}
        
        session("__SP_ADMIN_LOGIN_PAGE_SHOWED_SUCCESS__", true);
        return $this->fetch(":login");
	}
    
    public function doLogin()
    {
       	// 判断登录页面是否显示成功
        $loginAllowed = session("__SP_ADMIN_LOGIN_PAGE_SHOWED_SUCCESS__");
        if (empty($loginAllowed)) {
            $this->error('非法登录!', cmf_get_root() . '/');
        }
        
        /**
         * 登陆逻辑 ...
         */

        // 登陆成功
        if($result)
        {
            session('ADMIN_ID', $result["id"]);
            session('name', $result["user_login"]);
            
            session("__LOGIN_BY_CMF_ADMIN_PW__", null);
            session("__SP_ADMIN_LOGIN_PAGE_SHOWED_SUCCESS__", null);
            
            $this->success(lang('LOGIN_SUCCESS'), url("admin/Index/index"));
        }
    }
}
```