---
title: "在 Windows 上模拟 Azure IoT Edge | Microsoft Docs"
description: "在 Windows 的模拟设备上安装 Azure IoT Edge 运行时，并部署第一个模块"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 08c501b9132bb21f47f099725d1fad5556befb4c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>在 Windows 的模拟设备上部署 Azure IoT Edge - 预览

Azure IoT Edge 将云带来的价值转移至物联网 (IoT) 设备。 本教程将介绍如何创建生成传感器数据的模拟 IoT Edge 设备。 学习如何：

![教程体系结构][2]

你在本教程中创建的模拟设备是风力涡轮机上的监视器，它生成温度、湿度和压力数据。 涡轮机会根据天气条件表现出不同级别的工作效率，所以你对这些数据很感兴趣。 其他 Azure IoT Edge 教程均以本教程中通过部署模块（这些模块分析业务见解）执行的操作为基础。 

## <a name="prerequisites"></a>先决条件

本教程假设使用运行 Windows 的计算机或虚拟机来模拟物联网设备。 

>[!TIP]
>如果正在虚拟机中运行 Windows，则启用[嵌套虚拟化][lnk-nested]并分配至少 2GB 内存。 

1. 确保使用的是支持的 Windows 版本：
   * Windows 10 
   * Windows Server
2. 安装[适用于 Windows 的 Docker][lnk-docker] 并确保其正在运行。
3. 安装 [Windows 上的 Python 2.7][lnk-python] 并确保可以使用 pip 命令。
4. 运行以下命令，下载 IoT Edge 控制脚本。

   ```
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT Edge 可运行 Windows 容器或 Linux 容器。 要使用 Windows 容器，必须在基于 x64 的设备上运行：
>    * Windows 10 Fall Creators Update，或
>    * Windows Server 1709（内部版本 16299），或
>    * Windows IoT Core（内部版本 16299）
>
> 对于 Windows IoT Core，请按照[在 Windows IoT Core 上安装 IoT Edge 运行时][lnk-install-iotcore]中的说明进行操作。 否则，只能[将 Docker 配置为使用 Windows 容器][lnk-docker-containers]，并根据需要使用以下 PowerShell 命令验证先决条件：
>    ```
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>创建 IoT 中心

创建 IoT 中心，开始本教程的演练。
![创建 IoT 中心][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>注册 IoT Edge 设备

使用新创建的 IoT 中心注册 IoT Edge 设备。
![注册设备][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>配置 IoT Edge 运行时

在设备上安装并启动 Azure IoT Edge 运行时。 
![注册设备][5]

IoT Edge 运行时部署在所有 IoT Edge 设备上。 它由两个模块组成。 首先，IoT Edge 代理协助部署和监视 IoT Edge 设备上的模块。 其次，IoT Edge 中心管理 IoT Edge 设备模块之间以及设备和 IoT 中心之间的通信。 


使用以下步骤安装并启动 IoT Edge 运行时：

1. 使用上一节的 IoT Edge 设备连接字符串配置运行时。

   ```
   iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. 启动运行时。

   ```
   iotedgectl start
   ```

1. 检查 Docker，查看 IoT Edge 代理是否正作为模块运行。

   ```
   docker ps
   ```

## <a name="deploy-a-module"></a>部署模块

从云端管理 Azure IoT Edge 设备，部署将遥测数据发送到 IoT 中心的模块。
![注册设备][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>查看生成的数据

此快速入门中，创建了新的 IoT Edge 设备，并在该设备上安装了 IoT Edge 运行时。 然后，使用了 Azure 门户推送 IoT Edge 模块，使其在不更改设备本身的情况下在设备上运行。 这种情况下，推送的模块创建可用于本教程的环境数据。 

查看从 tempSensor 模块发送的消息：

```cmd/sh
sudo docker logs -f tempSensor
```

还可使用 [IoT 中心资源管理器工具][lnk-iothub-explorer]查看设备正在发送的遥测。 

## <a name="next-steps"></a>后续步骤

在本教程中，创建了新的 IoT Edge 设备，并使用 Azure IoT Edge 云接口将代码部署到该设备上。 现在，已有能生成其环境原始数据的模拟设备。 

本教程是其他所有 IoT Edge 教程的先决条件。 可以继续查看任何其他教程，了解 Azure IoT Edge 如何帮助将这些数据转化为边缘业务见解。

> [!div class="nextstepaction"]
> [将 C# 代码作为模块进行开发和部署](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md