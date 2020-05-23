
查找3个数据库的字段保留字，避免使用关键字

## 1、Pgsql:

```sql
/***********by liaoguohua 2017-06-07 17:27   质量保证*****************/
drop table if exists  qa_plan;
create table qa_plan(
  id integer not null,
  projectID integer ,
  status integer,
  qa varchar(200),
  title varchar(100) ,
  description varchar(200),
  planDate TIMESTAMP,
  actualDate TIMESTAMP,
  createBy integer,
  createTime TIMESTAMP,
  lastUpdateBy integer,
  lastUpdateTime TIMESTAMP,
  companyID integer not null,
  constraint pk_qa_plan primary key(id, companyID)
);
```


大文本：text

```sql
alter table alm_release add responser VARCHAR(500) default '';

ALTER TABLE st_codefunction ALTER COLUMN fid TYPE VARCHAR(200);


ALTER TABLE et_demandFunPointHisValue ADD CONSTRAINT pk_et_demandFunPointHisValue PRIMARY KEY (id,versionno,baseLine_versionNo, companyid);


ALTER TABLE et_demandFunPointHisValue ADD CONSTRAINT fk_et_demandFunPointHisValue FOREIGN KEY (formid,versionno,baseLine_versionNo, companyid) REFERENCES et_demandFunPointHis (id,versionno,baseLine_versionNo, companyid) ON DELETE CASCADE ON UPDATE NO ACTION;

alter table et_deviceapplicationitem drop column "name";

alter table dm_version drop constraint fk_dm_version;
```

## 2、Ms-sql 

```sql
if exists (select 1
            from  sysobjects
           where  id = object_id('qa_plan')
            and   type = 'U')
   drop table qa_plan
;
create table qa_plan(
  id integer not null,
  projectID integer ,
  status integer,
  qa nvarchar(200),
  title nvarchar(100) ,
  description nvarchar(200),
  planDate datetime,
  actualDate datetime,
  createBy integer,
  createTime datetime,
  lastUpdateBy integer,
  lastUpdateTime datetime,
  companyID integer not null,
  constraint pk_qa_plan primary key(id, companyID)
);
```

文本：ntext

```sql
alter table et_checktemplatepoint add  checkWay integer DEFAULT 0;

ALTER TABLE st_codefunction ALTER COLUMN fid NVARCHAR(200);

ALTER TABLE et_demandRevisedValue ADD CONSTRAINT fk_et_demandRevisedValue FOREIGN KEY (formid, companyid) REFERENCES et_demandRevised (id, companyid) ON DELETE CASCADE ON UPDATE NO ACTION;

ALTER TABLE et_demandFunPoint ADD  CONSTRAINT pk_et_demandFunPoint PRIMARY KEY (id, companyid);

alter table et_deviceapplicationitem drop column "name";

ALTER TABLE AGILE_SpringPlan ALTER COLUMN productworkItem nvarchar(200) 
GO


```

## 3、Oracle
所有的名称长度不能超过30

```sql
create table qa_plan(
  id integer not null,
  projectID integer ,
  status integer,
  qa nvarchar2(200),
  title nvarchar2(100) ,
  description nvarchar2(200),
  planDate TIMESTAMP,
  actualDate TIMESTAMP,
  createBy integer,
  createTime TIMESTAMP,
  lastUpdateBy integer,
  lastUpdateTime TIMESTAMP,
  companyID integer not null,
  constraint pk_qa_plan primary key(id, companyID)
);
```

文本：clob

```sql
alter table dm_attach add versionno integer default 0;

alter table alm_specinfo modify  responser  NVARCHAR2(500);

alter table pm_userlicense drop column id;

alter table pm_userlicense drop constraint "pk_pm_userlicense";

alter table pm_userlicense add constraint pk_pm_userlicense PRIMARY key (userid,productcode,companyid);

alter table wf_schemastatus add constraint fk_wf_status foreign key (schemaid, companyid) references fm_formschema (id,companyid) ON DELETE CASCADE;


INSERT INTO st_companyfunction (id, parentid, name, description, description1, status, type, policy, seqno, folderurl, clienturl, imageurl, companyid,appID)
VALUES (85, 0, 'PROJECT_COLLECTION', '项目群', 'Project Collection', 1, 0, 0,8, '', '/ProjectAction.do?operation=dashboard'||''||'&'||''||'isCollection=true', '/images/16x16/menu_project_manager.gif', 1,1);
```


