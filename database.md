CREATE TABLE `x_user` (`id` int(11) NOT NULL AUTO_INCREMENT,`username` varchar(100) NOT NULL,`password` varchar(100) DEFAULT NULL,`gid` varchar(20) DEFAULT NULL,`age` varchar(20) DEFAULT NULL,`sex` varchar(20) DEFAULT NULL,`email` varchar(50) DEFAULT NULL,`backup` varchar(500) DEFAULT NULL,`status` int(1) DEFAULT NULL,`deleted` INT(1) DEFAULT 0,PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

insert into `x_user` (`id`, `username`, `password`, `gid`,`age`,`sex`,`email`, `backup`, `status`, `deleted`) values('1','张三','123456','SA001','22','男','zs@163.com','软件学院一年一班','1','0');
insert into `x_user` (`id`, `username`, `password`, `gid`,`age`,`sex`,`email`, `backup`, `status`, `deleted`) values('2','李四','123456','SA002','22','男','ls@163.com','软件学院一年一班','1','0');
insert into `x_user` (`id`, `username`, `password`, `gid`,`age`,`sex`,`email`, `backup`, `status`, `deleted`) values('3','王五','123456','SA003','22','男','ww@163.com','软件学院一年一班','1','0');



CREATE TABLE `x_role` (`role_id` int(11) NOT NULL AUTO_INCREMENT,`role_name` varchar(50) DEFAULT NULL,PRIMARY KEY (`role_id`)) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4;

insert into `x_role` (`role_id`, `role_name`) values('1','admin');
insert into `x_role` (`role_id`, `role_name`) values('2','teacher');
insert into `x_role` (`role_id`, `role_name`) values('3','student');



CREATE TABLE `x_menu` (`menu_id` int(11) NOT NULL AUTO_INCREMENT,`component` varchar(100) DEFAULT NULL,`path` varchar(100) DEFAULT NULL,`redirect` varchar(100) DEFAULT NULL,`name` varchar(100) DEFAULT NULL,`title` varchar(100) DEFAULT NULL,`icon` varchar(100) DEFAULT NULL,`parent_id` int(11) DEFAULT NULL,`is_leaf` varchar(1) DEFAULT NULL,`hidden` tinyint(1) DEFAULT NULL,PRIMARY KEY (`menu_id`)) ENGINE=InnoDB AUTO_INCREMENT=12 DEFAULT CHARSET=utf8mb4;

insert  into `x_menu`(`menu_id`,`component`,`path`,`redirect`,`name`,`title`,`icon`,`parent_id`,`is_leaf`,`hidden`) values (1,'Layout','/user','/user/list','userManage','用户管理','userManage',0,'N',0),(2,'user/user','list',NULL,'userList','用户列表','userList',1,'Y',0),(3,'user/role','role',NULL,'roleList','角色列表','role',1,'Y',0),(4,'user/permission','permission',NULL,'permissionList','权限列表','permission',1,'Y',0);

CREATE TABLE `x_user_role` (`id` int(11) NOT NULL AUTO_INCREMENT,`user_id` int(11) DEFAULT NULL,`role_id` int(11) DEFAULT NULL,PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4;

insert into `x_user_role` (`id`, `user_id`, `role_id`) values('1','1','1');

CREATE TABLE `x_role_menu` (`id` int(11) NOT NULL AUTO_INCREMENT,`role_id` int(11) DEFAULT NULL,`menu_id` int(11) DEFAULT NULL,PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4;