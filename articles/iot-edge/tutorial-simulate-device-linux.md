---
title: "在 Linux 上模拟 Azure IoT Edge | Microsoft Docs"
description: "在 Linux 的模拟设备上安装 Azure IoT Edge 运行时，并部署第一个模块"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>在 Linux 的模拟设备上部署 Azure IoT Edge - 预览

Azure IoT Edge 使你可在设备上执行分析和数据处理，而无需推送所有数据到云。 IoT Edge 教程演示如何部署不同类型的模块（通过 Azure 服务或自定义代码生成），但是首先需要一个设备用以测试。 

本教程将介绍如何创建模拟 IoT Edge 设备，然后部署生成传感数据的模块。 学习如何：

![教程体系结构][2]

本教程中创建的模拟设备是一种监视器，它能生成温度、湿度和压力数据。 其他 Azure IoT Edge 教程均以本教程中通过部署模块（这些模块分析业务见解）执行的操作为基础。 

## <a name="prerequisites"></a>先决条件

本教程假设使用运行 Linux 的计算机或虚拟机来模拟物联网设备。 需要以下服务以成功部署 IoT Edge 设备：

- [安装适用于 Linux 的 Docker][lnk-docker-ubuntu] 并确保其正在运行。 
- 大多数 Linux 发行版（包括 Ubuntu）均已安装 Python 2.7。 使用以下命令确保已安装 pip：`sudo apt-get install python-pip`。

## <a name="create-an-iot-hub"></a>创建 IoT 中心

创建 IoT 中心，开始本教程的演练。
![创建 IoT 中心][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>注册 IoT Edge 设备

使用新创建的 IoT 中心注册 IoT Edge 设备。
![注册设备][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>安装和启动 IoT Edge 运行时

在设备上安装并启动 Azure IoT Edge 运行时。 
![注册设备][5]

IoT Edge 运行时部署在所有 IoT Edge 设备上。 它由两个模块组成。 首先，IoT Edge 代理协助部署和监视 IoT Edge 设备上的模块。 其次，IoT Edge 中心管理 IoT Edge 设备模块之间以及设备和 IoT 中心之间的通信。 

使用以下步骤安装并启动 IoT Edge 运行时：

1. 在即将运行 IoT Edge 设备的计算机上，下载 IoT Edge 控件脚本。

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. 使用上一节的 IoT Edge 设备连接字符串配置运行时。

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. 启动运行时。

   ```
   sudo iotedgectl start
   ```

1. 检查 Docker，查看 IoT Edge 代理是否正作为模块运行。

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>部署模块

从云端管理 Azure IoT Edge 设备，部署将遥测数据发送到 IoT 中心的模块。
![注册设备][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>查看生成的数据

此快速入门中，创建了新的 IoT Edge 设备，并在该设备上安装了 IoT Edge 运行时。 然后，使用了 Azure 门户推送 IoT Edge 模块，使其在不更改设备本身的情况下在设备上运行。 这种情况下，推送的模块创建可用于本教程的环境数据。 

查看从 tempSensor 模块发送的消息：

```cmd/sh
docker logs -f tempSensor
```

还可使用 [IoT 中心资源管理器工具][lnk-iothub-explorer]查看设备正在发送的遥测。 

## <a name="next-steps"></a>后续步骤

在本教程中，创建了新的 IoT Edge 设备，并使用 Azure IoT Edge 云接口将代码部署到该设备上。 现在，已有能生成其环境原始数据的模拟设备。 

本教程是其他所有 IoT Edge 教程的先决条件。 可以继续查看任何其他教程，了解 Azure IoT Edge 如何帮助将这些数据转化为边缘业务见解。

> [!div class="nextstepaction"]
> [将 C# 代码作为模块进行开发和部署](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
