

mysqldump -h db-amavisd-ro.mailroute.net -u admin --ignore-table=mroute_admin.audittrail_historyrecord --ignore-table=mroute_admin.amavisd_gsuitesyncresult --ignore-table=mroute_admin.amavisd_ldapsyncresult --ignore-table=mroute_admin.amavisd_o365syncresult  --ignore-table=mroute_admin.reversion_revision  --ignore-table=mroute_admin.reversion_version  -p mroute_admin > /tmp/mroute_admin.sql






CREATE TABLE `audittrail_historyrecord` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `created_at` datetime(6) NOT NULL,
  `user_id` int(11) DEFAULT NULL,
  `username` longtext,
  `obj_id` int(11) NOT NULL,
  `user_backend` longtext,
  `email_account_id` int(11) DEFAULT NULL,
  `domain_id` int(11) DEFAULT NULL,
  `customer_id` int(11) DEFAULT NULL,
  `reseller_id` int(11) DEFAULT NULL,
  `type` smallint(6) NOT NULL,
  `diff` longtext,
  `content_type_id` int(11) NOT NULL,
  `revision_id` int(11) DEFAULT NULL,
  `obj_repr` longtext,
  PRIMARY KEY (`id`),
  KEY `audittrail_hi_content_type_id_638f248c_fk_django_content_type_id` (`content_type_id`),
  KEY `audittrail_history_revision_id_8a94b205_fk_reversion_revision_id` (`revision_id`),
  KEY `audittrail_historyrecord_418d810b` (`email_account_id`),
  KEY `audittrail_historyrecord_662cbf12` (`domain_id`),
  CONSTRAINT `audittrail_hi_content_type_id_638f248c_fk_django_content_type_id` FOREIGN KEY (`content_type_id`) REFERENCES `django_content_type` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=23756498 DEFAULT CHARSET=utf8;

CREATE TABLE `amavisd_gsuitesyncresult` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `test_run` tinyint(1) NOT NULL,
  `created_at` datetime(6) NOT NULL,
  `result` longtext NOT NULL,
  `errors` longtext,
  `source_data` longtext,
  `added_accounts` int(11) DEFAULT NULL,
  `updated_accounts` int(11) DEFAULT NULL,
  `deleted_accounts` int(11) DEFAULT NULL,
  `added_aliases` int(11) DEFAULT NULL,
  `updated_aliases` int(11) DEFAULT NULL,
  `deleted_aliases` int(11) DEFAULT NULL,
  `gsuitesync_id` int(11) NOT NULL,
  `user_id` int(11) DEFAULT NULL,
  `status` smallint(6) NOT NULL,
  `task_id` longtext NOT NULL,
  PRIMARY KEY (`id`),
  KEY `amavisd_gsuitesyncre_gsuitesync_id_a21774db_fk_amavisd_g` (`gsuitesync_id`),
  KEY `amavisd_gsuitesyncresult_user_id_bc4a3783_fk_auth_user_id` (`user_id`),
  CONSTRAINT `amavisd_gsuitesyncre_gsuitesync_id_a21774db_fk_amavisd_g` FOREIGN KEY (`gsuitesync_id`) REFERENCES `amavisd_gsuitesync` (`id`),
  CONSTRAINT `amavisd_gsuitesyncresult_user_id_bc4a3783_fk_auth_user_id` FOREIGN KEY (`user_id`) REFERENCES `auth_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=27055 DEFAULT CHARSET=utf8;


CREATE TABLE `amavisd_ldapsyncresult` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `ldapsync_id` int(11) NOT NULL,
  `user_id` int(11) DEFAULT NULL,
  `test_run` tinyint(1) NOT NULL,
  `created_at` datetime NOT NULL,
  `result` longtext NOT NULL,
  `errors` longtext,
  `source_data` longtext,
  `added_accounts` int(11) DEFAULT NULL,
  `added_aliases` int(11) DEFAULT NULL,
  `deleted_accounts` int(11) DEFAULT NULL,
  `deleted_aliases` int(11) DEFAULT NULL,
  `updated_accounts` int(11) DEFAULT NULL,
  `updated_aliases` int(11) DEFAULT NULL,
  `status` smallint(6) NOT NULL,
  `task_id` longtext NOT NULL,
  PRIMARY KEY (`id`),
  KEY `amavisd_ldapsyncresult_96dc7f62` (`ldapsync_id`),
  KEY `amavisd_ldapsyncresult_fbfc09f1` (`user_id`),
  CONSTRAINT `amavisd_ldapsyncresult_user_id_817f7b0d_fk_auth_user_id` FOREIGN KEY (`user_id`) REFERENCES `auth_user` (`id`),
  CONSTRAINT `ldapsync_id_refs_id_c55297e2` FOREIGN KEY (`ldapsync_id`) REFERENCES `amavisd_ldapsync` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=383261 DEFAULT CHARSET=utf8;

CREATE TABLE `amavisd_o365syncresult` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `test_run` tinyint(1) NOT NULL,
  `created_at` datetime(6) NOT NULL,
  `result` longtext NOT NULL,
  `errors` longtext,
  `source_data` longtext,
  `added_accounts` int(11) DEFAULT NULL,
  `updated_accounts` int(11) DEFAULT NULL,
  `deleted_accounts` int(11) DEFAULT NULL,
  `added_aliases` int(11) DEFAULT NULL,
  `updated_aliases` int(11) DEFAULT NULL,
  `deleted_aliases` int(11) DEFAULT NULL,
  `o365sync_id` int(11) NOT NULL,
  `user_id` int(11) DEFAULT NULL,
  `status` smallint(6) NOT NULL,
  `task_id` longtext NOT NULL,
  PRIMARY KEY (`id`),
  KEY `amavisd_o365syncresu_o365sync_id_dee7ff31_fk_amavisd_o` (`o365sync_id`),
  KEY `amavisd_o365syncresult_user_id_59023a9e_fk_auth_user_id` (`user_id`),
  CONSTRAINT `amavisd_o365syncresu_o365sync_id_dee7ff31_fk_amavisd_o` FOREIGN KEY (`o365sync_id`) REFERENCES `amavisd_o365sync` (`id`),
  CONSTRAINT `amavisd_o365syncresult_user_id_59023a9e_fk_auth_user_id` FOREIGN KEY (`user_id`) REFERENCES `auth_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=528244 DEFAULT CHARSET=utf8;


CREATE TABLE `reversion_revision` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `date_created` datetime(6) NOT NULL,
  `comment` longtext NOT NULL,
  `user_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `reversion_revision_user_id_17095f45_fk_auth_user_id` (`user_id`),
  KEY `reversion_revision_c69e55a4` (`date_created`),
  CONSTRAINT `reversion_revision_user_id_17095f45_fk_auth_user_id` FOREIGN KEY (`user_id`) REFERENCES `auth_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8256042 DEFAULT CHARSET=utf8;

CREATE TABLE `reversion_version` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `object_id` varchar(191) NOT NULL,
  `format` varchar(255) NOT NULL,
  `serialized_data` longtext NOT NULL,
  `object_repr` longtext NOT NULL,
  `content_type_id` int(11) NOT NULL,
  `revision_id` int(11) NOT NULL,
  `db` varchar(191) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `reversion_version_db_b2c54f65_uniq` (`db`,`content_type_id`,`object_id`,`revision_id`),
  KEY `reversion_ver_content_type_id_7d0ff25c_fk_django_content_type_id` (`content_type_id`),
  KEY `reversion_version_revision_id_af9f6a9d_fk_reversion_revision_id` (`revision_id`),
  KEY `reversion_v_content_f95daf_idx` (`content_type_id`,`db`),
  CONSTRAINT `reversion_ver_content_type_id_7d0ff25c_fk_django_content_type_id` FOREIGN KEY (`content_type_id`) REFERENCES `django_content_type` (`id`),
  CONSTRAINT `reversion_version_revision_id_af9f6a9d_fk_reversion_revision_id` FOREIGN KEY (`revision_id`) REFERENCES `reversion_revision` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=14570776 DEFAULT CHARSET=utf8;