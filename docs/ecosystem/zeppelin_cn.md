---
layout: page
title: "Install"
description: "This page will help you get started and will guide you through installing Apache Submarine and running it in the command line."
group: quickstart
---
<!--
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
{% include JB/setup %}

# Submarine Integration Zeppelin [English version](zeppelin.html)

Hadoop Submarine 是 Hadoop 3.1 版本中最新的机器学习框架子项目。 它允许 Hadoop 支持 Tensorflow，PyTroch，MXNet，Caffe 等各种深度学习框架，进行机器学习算法开发，分布式模型训练，模型管理和模型发布提供了全功能的系统框架，并结合了hadoop的内在数据存储和数据处理功能使数据科学家能够更好地挖掘和获取数据的价值。

深度学习算法项目需要数据采集，数据处理，数据清理，交互式可视化编程调整参数，算法测试，算法发布，算法作业调度，离线模型训练，模型在线服务等很多处理过程。 Zeppelin 是一款基于 WEB 的 Notebook 支持交互式数据分析。 您可以使用 SQL，Scala，Python 等语法进行交互式的数据开发工作。

您可以使用 zeppelin 中 20 多种解释器（例如：spark，hive，Cassandra，Elasticsearch，Kylin，HBase等）对 Hadoop 中的数据进行数据提取，数据清理，特征提取等数据预处理过程。通过将 Zeppelin 集成到 Submarine 中，我们可以使用 zeppelin 的数据发现，数据分析和数据可视化以及协作功能，完成机器学习模型训练期间的可视化算法开发和参数调整工作。

## Architecture

![submarine-architecture]({{BASE_PATH}}/assets/themes/submarine/img/ecosystem/submarine-architecture.png)

如上图所示，从系统架构上阐述了 Submarine 如何通过 Zeppelin 进行机器学习算法的开发和模型训练。

在安装部署完 Hadoop 3.1+ 和 Zeppelin 后，Submarine 将为每个用户在 YARN 中创建一个完全独立的 Zeppelin Submarine interpreter Docker 容器，这个容器中包含了 Tensorflow 的开发和运行环境，Zeppelin Server 通过连接 YARN 中的 Zeppelin Submarine interpreter Docker 容器，让算法工程师可以在 Zeppelin Notebook 中进行 Tensorflow 的单机环境下的算法开发和数据可视化展现。

在完成算法开发后，算法工程师可以直接在 Zeppelin 中将算法以 JOB 的方式提交到 YARN 中进行模型的离线分布式训练，通过 Submarine 为每个算法工程师提供的 TensorBoard 进行模型训练的实时展示。

你不仅仅可以完成算法的模型训练，你还可以借助 Zeppelin 中原有的二十多种解释器，完成模型的数据预处理工作，例如：你可以在算法 Note 中通过 Zeppelin 中的 Spark 解释器进行数据抽取、过滤和特征提取等操作。

未来，你还可以借助 Zeppelin 即将发布的 Workflow 工作流编排服务，你可以在一个 Note 中完成 Spark、Hive 的数据处理和 Tensorflow 模型训练，通过可视化等方式编排成一个工作流，在生产环境中进行作业的周期性调度。

## Zeppelin Submarine interpreter

![submarine-architecture]({{BASE_PATH}}/assets/themes/submarine/img/ecosystem/submarine-interpreter.png)

如上图所示，从内部实现上阐述了 Submarine 如何结合 Zeppelin 进行机器学习算法的开发和模型训练。

1. 算法工程师在 Zeppelin 中通过使用 Submarine interpreter 创建一个 Tensorflow 的 notebook（左图）。

   需要注意的是你需要在一个 Note 中完成整个算法的开发。

2. 你可以在 Note 中的一些段落中使用例如 Spark 进行数据预处理工作。

3. 在 notebook 的其他段落中使用 Python 进行 Tensorflow 的算法开发和调试，Submarine 会在 YARN 中为你创建 Zeppelin Submarine Interpreter Docker Container，容器中包含了以下功能和服务：

   + **Shell 命令行工具**：让你可以查看 Zeppelin Submarine Interpreter Docker Container 中的系统环境，安装自己需要的扩展工具或是 Python 依赖库。
   + **Kerberos 库**：让你可以进行 kerberos 认证，访问启用了 Kerberos 认证的 Hadoop 集群。
   + **Tensorflow 运行环境**：让你可以进行 tensorflow 算法代码的开发。
   + **Python 运行环境**：让你可以进行 tensorflow 代码的开发。
   + 在 Zeppelin 中通过一个 Note 来完成一个完整的算法开发，如果这个算法包含了多个模块，你可以在 Note 中通过多个段落分别编写不同的算法模块，每个段落的标题就是算法模块的名称，段落的内容就是这个算法模块的代码内容。
   + **HDFS Client**：Zeppelin Submarine Interpreter 会自动将你在 Note 中编写的算法代码提交到 HDFS 中。

   **Submarine interpreter Docker Image** 是由 Submarine 为你提供支持 Tensorflow （CPU 和 GPU 版本）的镜像文件，并安装了 Python 常用的算法库，你也可以在 Submarine 提供的基础镜像之上安装你所需要的其他开发依赖库进行使用。

4. 当你完成算法模块的开发后，你可以在 Note 中新建一个段落输入 `%submarine dashboard` 后执行， Zeppelin 将会创建出 Submarine Dashboard ，在控制面板中通过选择 `JOB RUN` 命令选项就可以将这个 Note 中编写的机器学习算法作为一个 JOB 提交到 YARN 中，创建出 Tensorflow Model Training Docker Container ，容器中包含了以下部分：

   + Tensorflow 运行环境。
   + HDFS Client 会自动完成从 HDFS 中下载算法文件 Mount 到容器中进行分布式模型训练，并将算法文件 Mount 到容器的 Work Dir 路径。

   **Submarine Tensorflow Docker Image** 是有 Submarine 为你提供支持 Tensorflow （CPU 和 GPU 版本）的镜像文件，并安装了 Python 常用的算法库，你也可以在 Submarine 提供的基础镜像之上安装你所需要的其他开发依赖库进行使用。

### Submarine shell（Optional）

在 Zeppelin 中使用 Submarine Interpreter 创建 Note 之后，如果有需要你可以在 Note 中新增段落，使用 `%submarine.sh` 标识符，你可以使用 Shell 命令对  Submarine Interpreter Docker Container 进行各种操作，例如：

1. 查看 Container 中的 Pythone 版本
2. 查看 Container 的系统环境
3. 安装你自己需要的依赖库
4. 通过 kinit 进行 kerberos 认证
5. 使用 Container 中的 Hadoop 进行 HDFS 操作等等

### Submarine python

你可以在 Note 中新增一个或多个段落，使用 `%submarine.python` 标识符，使用 Python 编写 Tensorflow 的算法模块。

### Submarine Dashboard

在通过使用 `%submarine.python` 编写完 Tensorflow 算法后，你可以在 Note 中新增一个段落，输入 `%submarine dashboard` 并执行，Zeppelin 将会创建出 Submarine 的 Dashboard。

![](assets/Submarine-Dashboard.gif)

通过 Submarine Dashboard 你可以完成对 Submarine 的所有操作控制，例如：

1. Usage：显示 Submarine 的命令说明，帮助开发人员进行问题定位。

2. Refresh：Zeppelin 将会清除你在 Dashboard 中的所有输入内容。

3. Tensorboard：将会跳转到 Submarine 为每个用户创建的 Tensorboard WEB 系统，通过 Tensorboard 你可以实时查看 Tensorflow 模型训练的实时状态。

4. Command

   + JOB RUN：选择 `JOB RUN` 将会显示提交 JOB 的参数输入界面

    | 参数名称          | 说明                                                         |
    | ----------------- | ------------------------------------------------------------ |
    | Checkpoint Path   | Submarine 为每个用户的每个 Note 设置了一个单独的 Tensorflow 训练的 Checkpoint 路径，保存了这个 Note 历史的训练数据，用于训练模型数据的输出，Tensorboard 使用这个路径中的数据进行模型展示，用户不可修改。例如：`hdfs://cluster1/...` ，Checkpoint Path 的环境变量名称是 `%checkpoint_path%`，你可以在 `PS Launch Cmd` 和 `Worker Launch Cmd` 中直接使用 `%checkpoint_path%` 代替在 Checkpoint Path 中的输入值 |
    | Data Path         | 用户指定 Tensorflow 算法的数据数据目录，只支持 HDFS 的目录。Data Path 的环境变量名称是 `%input_path%`，你可以在 `PS Launch Cmd` 和 `Worker Launch Cmd` 中直接使用 `%input_path%` 代替在 Data Path 中的输入值 |
    | PS Launch Cmd     | Tensorflow Parameter services launch command，例如：`python cifar10_main.py --data-dir=%input_path% --job-dir=%checkpoint_path% --num-gpus=0 ...` |
    | Worker Launch Cmd | Tensorflow Worker services launch command，例如：`python cifar10_main.py --data-dir=%input_path% --job-dir=%checkpoint_path% --num-gpus=1 ...` |

   + JOB STOP

     你可以选择执行 `JOB STOP` 命令，停止一个已经提交和正在运行的 Tensorflow 模型训练任务

   + TENSORBOARD START

     你可以选择执行 `TENSORBOARD START` 命令，创建你的 TENSORBOARD Docker Container。

   + TENSORBOARD STOP

     你可以选择执行 `TENSORBOARD STOP` 命令，停止并销毁你的 TENSORBOARD Docker Container。

5. Run Command：执行你所选择的操作命令
6. Clean Chechkpoint：勾选上这个选项，在每次执行 `JOB RUN` 之前，将会清除这个 Note 的 Checkpoint Path 中的数据。

### Submarine interpreter Attributes

Zeppelin Submarine interpreter 提供了以下属性对 Submarine 解释器进行定制

| 属性名                             | 属性值                       | 说明                                                         |
| ---------------------------------- | ---------------------------- | ------------------------------------------------------------ |
| `DOCKER_HADOOP_HDFS_HOME`            | 例如：/hadoop-3.1            | 在以下3个镜像中的 Hadoop 路径（SUBMARINE_INTERPRETER_DOCKER_IMAGE、tf.parameter.services.docker.image、tf.worker.services.docker.image） |
| `DOCKER_JAVA_HOME`                   | 例如：/opt/java              | 在以下3个镜像中的 JAVA 路径（SUBMARINE_INTERPRETER_DOCKER_IMAGE、tf.parameter.services.docker.image、tf.worker.services.docker.image） |
| `HADOOP_YARN_SUBMARINE_JAR`          |                              | 在 Zeppelin 服务器中安装的 Hadoop-3.1+ 版本中的 Submarine JAR 包的路径 |
| `INTERPRETER_LAUNCH_MODE`            | local/yarn                   | 将 Submarine 解释器实例运行在 local 或 YARN 中 local 主要用于 submarine interpreter 的开发和调试 YARN 模式用于生产环境 |
| `SUBMARINE_HADOOP_CONF_DIR`          |                              | 设置 HADOOP-CONF 路径，可以用于支持多个hadoop 集群环境的情况 |
| `SUBMARINE_HADOOP_HOME`              |                              | 在 Zeppelin 服务器中安装的 Hadoop-3.1+ 以上版本路径          |
| `SUBMARINE_HADOOP_KEYTAB`            |                              | 用于开启了 kerberos 认证的 hadoop 集群的 keytab 文件路径     |
| `SUBMARINE_HADOOP_PRINCIPAL`         |                              | 用于开启了 kerberos 认证的 hadoop 集群的 keytab 文件的 PRINCIPAL 信息 |
| `SUBMARINE_INTERPRETER_DOCKER_IMAGE` |                              | 在 INTERPRETER_LAUNCH_MODE=yarn 时，Submarine 会使用这个镜像创建 Zeppelin Submarine interpreter 容器为用户创建算法开发环境 |
| `docker.container.network`           |                              | YARN 的 Docker 网络名称                                      |
| `machinelearing.distributed.enable`  |                              | 是否使用分布式模式 JOB RUN 提交的模型训练                    |
| `shell.command.timeout.millisecs`    | 60000                        | 在 Submarine interpreter 容器中执行 Shell 命令的超时设置     |
| `submarine.algorithm.hdfs.path`      |                              | 将使用 Submarine interpreter 开发的机器学习算法以文件的方式保存到 HDFS 中 |
| `submarine.yarn.queue`               | root.default                 | Submarine 提交模型训练的 YARN 队列名称                       |
| `tf.checkpoint.path`                 |                              | Tensorflow checkpoint 路径，每个用户都会在这个路径下使用用户名创建用户的 checkpoint 二级路径，用户提交的每个算法都会使用note id 创建checkpoint 三级路径（用户的 Tensorboard 使用这个路径里的 checkpoint 数据进行可视化展示） |
| `tf.parameter.services.cpu`          |                              | Submarine 提交模型分布式训练时为 Tensorflow parameter services 申请的 CPU 核数 |
| `tf.parameter.services.docker.image` |                              | Submarine 提交模型分布式训练时创建 Tensorflow parameter services 使用的镜像 |
| `tf.parameter.services.gpu`          |                              | Submarine 提交模型分布式训练时为 Tensorflow parameter services 申请的 GPU 核数 |
| `tf.parameter.services.memory`       | 2G                           | Submarine 提交模型分布式训练时为 Tensorflow parameter services 申请的 Memory 资源 |
| `tf.parameter.services.num`          |                              | Submarine 提交模型分布式训练时使用的 Tensorflow parameter services 数目 |
| `tf.tensorboard.enable`              | true                         | 为每个用户创建一个独立的 Tensorboard                         |
| `tf.worker.services.cpu`             |                              | Submarine 提交模型训练时为 Tensorflow worker services 申请的 CPU 资源 |
| `tf.worker.services.docker.image`    |                              | Submarine 提交模型分布式训练时创建 Tensorflow worker services 使用的镜像 |
| `tf.worker.services.gpu`             |                              | Submarine 提交模型训练时为 Tensorflow worker services 申请的 GPU 资源 |
| `tf.worker.services.memory`          |                              | Submarine 提交模型训练时为 Tensorflow worker services 申请的 CPU 资源 |
| `tf.worker.services.num`             |                              | Submarine 提交模型分布式训练时使用的 Tensorflow worker services 数目 |
| `yarn.webapp.http.address`           | 例如：http://hadoop-3-1:8088 | YARN web ui address                                          |
| `zeppelin.interpreter.rpc.portRange` | 29914                        | 需要在 SUBMARINE_INTERPRETER_DOCKER_IMAGE 配置的镜像中 export 这个端口，用于 Zeppelin Server 和 Submarine interpreter 容器进行 RPC 通讯 |
| `zeppelin.ipython.grpc.message_size` | 33554432                     | Submarine interpreter 容器中 IPython grpc 的消息大小设置     |
| `zeppelin.ipython.launch.timeout`    | 30000                        | Submarine interpreter 容器中 IPython 执行超时设置            |
| `zeppelin.python`                    | python                       | Submarine interpreter 容器中 python 的执行路径               |
| `zeppelin.python.maxResult`          | 10000                        | 从 Submarine interpreter 容器中返回 python 执行结果的最大条数 |
| `zeppelin.python.useIPython`         | false                        | 目前不支持IPython，必须为false                               |
| `zeppelin.submarine.auth.type`       | simple/kerberos              | Hadoop 是否开启了 kerberos 认证                              |


## 更多介绍

Youtube Submarine Channel：[https://www.youtube.com/channel/UC4JBt8Y8VJ0BW0IM9YpdCyQ](https://www.youtube.com/channel/UC4JBt8Y8VJ0BW0IM9YpdCyQ)
