# MongoDB 数据库简单操作

### 安装 MongoDB 数据库

介绍如何在系统中安装 MongoDB 数据库，运行下面安装命令：

```
npm install mongodb
```

若上面命令报错，没有权限，则运行下面命令：
```
sudo brew update
sudo brew install mongodb
```
请参考文档在 OS X 系统中安装 MongoDB，另外在该篇文档的左侧边栏中也有关于 MongoDB 在其它系统中的安装文档，若您不使用苹果机，可以参考一下。

### 运行 MongoDB

通过上述命令把 MongoDB 数据库安装成功之后，其实 MongoDB 数据库还不能使用，我们还得让它运行起来。在启动 MongoDB 数据库之前，我们需要创建一个用来存储 MongoDB 数据的目录，MongoDB 数据的默认存放位置是 /data/db （也需要手动创建）目录下，也可以更改为其它地方，比如说当前用户目录下，新建一个 data/db 目录用来存放 MongoDB 的数据：

```
cd ~
mkdir -p data/db
```
### 启动 MongoDB 数据库

目录 data 创建成功之后，就可以启动 MongoDB 数据库了：

```
mongod --dbpath ~/data/db
```
若上述命令执行之后不会退出，一直处于运行状态，说明 MongoDB 数据库可以使用了。查看 mongod 命令的帮助文档，可以在命令行中输入命令：

```
mongod -h
```

则在终端中打印出 mongod 命令各个选项的用法， 其中 --dbpath 选项就是指定数据存储路径

### 使用 MongoDB

既然 MongoDB 已经启动了，那如何操作它呢？通过 MongoDB 提供的 mongo shell 工具，可以很方便的和 MongoDB 数据库进行通信。

首先确保在 MongoDB 数据库运行的状态下，才能启动 mongo shell，在命令行中输入命令：

```
mongo
```

进入一个 shell 运行环境，在这里面可以调用 MongoDB 提供的接口操作数据库中存储的数据。下面先学几个简单的操作命令：

#### 列出所有的数据库名称
```
show dbs
```
#### 创建新的数据库

比如本项目用的数据库名为 react-express-api ：

```
use react-express-api
```

但是， react-express-api 数据库并不存在，只有当数据存入数据库时候才会真正的创建数据库

#### 创建新的 collection

在 react-express-api 数据库中创建一个新的 collection, 比如本项目用到的集合名为 posts:

```
db.createCollection('posts')
```

#### posts 集合中存入数据，posts 集合包含 category、title 和 content 三个字段

```
db.posts.insert({category: 'db', title: 'learning mongodb', content: 'mongodb is a nosql database'})
```

#### 查找 posts 集合中的所有记录

```
db.posts.find()
```

#### 更新 posts 集合中的一条记录

```
db.posts.update({_id: ObjectId('xxx')}, {$set: {title: 'mongodb'}})
```

#### 删除 posts 集合中的一条记录

```
db.posts.remove({_id: ObjectId('xxx')})
```

#### 删除 posts 集合中的所有记录

```
db.posts.remove({})
```

#### 删除数据库 react-express-api

```
use react-express-api
db.dropDatabase()
```
