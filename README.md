# VMware 虚拟机自动化部署工具

这是一个基于Ansible的自动化工具，用于在VMware vSphere环境中部署虚拟机并自动加入域。

## 功能特性

- 从模板自动部署VMware虚拟机
- 自定义虚拟机配置（CPU、内存、网络等）
- 自动加入域
- 支持多环境部署（开发、测试、生产）
- 支持多地域部署
- 支持 Windows 和 SUSE Linux 操作系统

## 前置条件

1. 软件要求：
   - Ansible Core 2.12或更高版本
   - Python 3.8或更高版本
   - pip包管理器
   - YaST2（用于SUSE系统配置）

2. 环境要求：
   - 可用的VMware vCenter环境
   - Windows模板镜像（已配置好基础设置）
   - SUSE Linux模板镜像（已完成基础配置）
   - 域控制器（用于加入域）

3. 网络要求：
   - 能够访问vCenter服务器
   - 部署机器需要能够访问目标虚拟机网络

## 安装步骤

1. 克隆本仓库：
   ```bash
   git clone <repository_url>
   ```

2. 安装所需的Ansible集合：
   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

## 配置说明

1. 在`group_vars/all.yml`中配置以下变量：
   - vCenter连接信息
   - 虚拟机默认配置
   - 域管理员账号信息
   - SUSE订阅管理信息
   - YaST2自动化配置参数

2. 在`host_vars/vsphere.yml`中配置特定的vSphere环境变量

3. 在`inventory/hosts.yml`中配置vCenter连接信息

注意：建议使用Ansible Vault加密敏感信息（如密码）：
```bash
ansible-vault encrypt_string --ask-vault-password
```

## 使用方法

1. 运行部署playbook：
   ```bash
   ansible-playbook vmware_provision.yml
   ```

2. 根据提示输入以下信息：
   - 环境类型（dev/test/prod）
   - 部署位置（如：sh/sz）
   - 域名
   - 操作系统类型（Windows/SUSE）
   - SUSE版本（如：15 SP4）

## 目录结构说明

```
.
├── files/                    # 静态文件
├── group_vars/              # 组变量
│   └── all.yml             # 通用配置变量
├── host_vars/              # 主机变量
│   └── vsphere.yml         # vSphere特定配置
├── inventory/              # 资产清单
│   └── hosts.yml          # 主机配置
├── roles/                  # 角色定义
│   └── vmware/            # VMware部署角色
├── schemas/               # JSON Schema定义
├── requirements.yml       # 依赖配置
└── vmware_provision.yml   # 主playbook

## 最佳实践

- 所有密码和敏感信息都应使用Ansible Vault加密
- 在生产环境部署前，建议在测试环境进行验证
- 定期更新虚拟机模板以包含最新的补丁
- 使用有意义的命名约定进行虚拟机命名
- 确保虚拟机模板已完成基础配置

## 故障排除

1. 如果部署失败，检查：
   - vCenter连接信息是否正确
   - 模板是否可用
   - 网络连接是否正常
   - 域控制器是否可达
   - SUSE订阅是否激活（针对SUSE系统）
   - YaST2配置是否正确（针对SUSE系统）

2. 常见错误：
   - 模板不存在
   - 网络配置错误
   - 域加入失败
   - 资源不足
   - SUSE注册失败
   - YaST2自动配置失败

## 维护与支持

如需帮助或报告问题，请创建Issue或联系管理员。