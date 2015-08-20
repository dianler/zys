yaf项目快速开发，集成了db操作类支持链式操作，支持读写分离，pdo，mysqli，mongo，upload操作，加入redis操作以及微信分享，随后会加入微信支付
===================================
###db操作类读写分离配置如下：
	;数据库驱动类型
	database.config.type='mysql'
	;服务器地址
	database.config.host='192.168.0.1,192.168.0.2'
	;数据库名
	database.config.name='root'
	;用户名
	database.config.user='user1,user2'
	;密码
	database.config.pwd='pwd1,pwd2'
	;端口
	database.config.port= '3306'
	;启用字段缓存
	database.config.fields_cache=false
	;数据库编码默认采用utf8
	database.config.charset='UTF-8'
	;数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
	database.config.deploy_type=1
	;数据库读写是否分离 主从式有效
	database.config.rw_separate=true
	;读写分离后 主服务器数量
	database.config.master_num=1
	;指定从服务器序号
	database.config.slave_no=''
###db操作类使用方法如下：
	  $where=array('id' =>37936);
   	  $user=new HbModel('hb_users');//直接实例化给表名就行了，其他跟操作thinkphp一样
	  $result=$user->where($where)->select();//支持链式操作，支持pdo，mysqli，mongo
	  echo $user->getlastsql();
	  print_r($result);
	  exit;
###redis操作使用方法如下：
          $this->_redis=new phpredis();//需要安装redis扩展
          $this->_redis->set('token',1);
###upload操作使用方法如下：
            $config=Yaf_Application::app()->getConfig()->upload->config->toArray();
            $ftpconfig=Yaf_Application::app()->getConfig()->ftp->config->toArray();
            $upload=new Upload($config, 'Ftp',$ftpconfig); 
            $info=$upload->upload();
            if (!$info) {// 上传错误提示错误信息
                echo $upload->getError();
            } else {// 上传成功
                if (!empty($info["UpLoadFile"]))
                    $pic=array("cate_pic" => $info["UpLoadFile"]['savepath'] . $info["UpLoadFile"]['savename']);
                print_r($pic);
            }
###微信分享操作使用方法如下：
            //微信分享
        $jssdk=new JSSDK("你的appId", "你的appSecret");
        $signPackage=$jssdk->GetSignPackage();;
        $data['appId']=$signPackage['appId'];
        $data['nonceStr']=$signPackage['nonceStr'];
        $data['timestamp']=$signPackage['timestamp'];
        $data['signature']=$signPackage['signature'];
        $this->getView()->assign("token", json_encode($data));
### 以上具体的使用方法，控制器里都有，随后会加入更多功能，有什么问题可以及时联系我 qieangel@hotmail.com