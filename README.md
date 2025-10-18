# 🚀 Zabbix Test Demo Project

[![Zabbix Version](https://img.shields.io/badge/Zabbix-7.0_LTS-red)](https://www.zabbix.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-blue)](https://www.docker.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Development-yellow)](https://github.com/yourusername/zabbix-test-demo)

## 📋 Overview

โปรเจค Zabbix Test Demo สำหรับการเรียนรู้และทดสอบระบบ Monitoring ด้วย Zabbix บน VPS Digital Ocean โดยใช้ Docker Compose เพื่อให้ง่ายต่อการติดตั้งและจัดการ

### 🎯 วัตถุประสงค์
- 🔍 ศึกษาและเรียนรู้ระบบ Zabbix Monitoring
- 🐳 ฝึกใช้ Docker Compose ในการ Deploy services
- 📊 ทดสอบการ Monitor ระบบต่างๆ
- 📚 สร้างเอกสารและคู่มือการใช้งานภาษาไทย

### ⚠️ คำเตือน
> **โปรเจคนี้สำหรับการเรียนรู้และทดสอบเท่านั้น** ไม่แนะนำให้ใช้ใน Production Environment

---

## 🔧 System Requirements

### Minimum Requirements (สำหรับทดสอบ)
- **OS:** Ubuntu 20.04 LTS หรือใหม่กว่า
- **CPU:** 2 vCPU
- **RAM:** 2 GB
- **Storage:** 50 GB
- **Docker:** version 24.0+
- **Docker Compose:** version 2.20+

### Recommended Requirements (สำหรับใช้งานจริง)
- **OS:** Ubuntu 22.04/24.04 LTS
- **CPU:** 4 vCPU
- **RAM:** 8 GB+
- **Storage:** 100 GB+ (SSD recommended)
- **Network:** Public IP with open ports (80, 443, 10051)

---

## 📦 Stack Components

| Component | Version | Purpose | Port |
|-----------|---------|---------|------|
| **Zabbix Server** | 7.0 LTS | Core monitoring engine | 10051 |
| **Zabbix Web** | 7.0 LTS | Web interface (Nginx + PHP) | 8080 |
| **PostgreSQL** | 15 | Database server | 5432 |
| **Nginx** | Latest | Web server | 80/443 |

---

## 🚀 Quick Start

### 1️⃣ Clone Repository
```bash
# Clone จาก git-rmutp
git clone https://git-rmutp.ac.th/yourusername/Zabbix-Test-Demo.git
cd Zabbix-Test-Demo
```

### 2️⃣ Setup Environment
```bash
# Copy environment template
cp .env.example .env

# Edit environment variables
nano .env
```

### 3️⃣ Start Services
```bash
# Using script
./scripts/start.sh

# Or using docker-compose directly
docker-compose -f docker/docker-compose.yml up -d
```

### 4️⃣ Access Zabbix
- **URL:** http://your-server-ip:8080
- **Username:** Admin
- **Password:** (ดูใน .env file)

---

## 📁 Project Structure

```
Zabbix-Test-Demo/
├── docker/                 # Docker configuration
│   └── docker-compose.yml  # Main compose file
├── config/                 # Service configurations
│   ├── zabbix/            # Zabbix configs
│   ├── postgres/          # PostgreSQL configs
│   └── nginx/             # Nginx configs
├── scripts/               # Management scripts
│   ├── setup/            # Setup utilities
│   ├── backup/           # Backup scripts
│   └── monitoring/       # Health check scripts
├── data/                  # Persistent data (git-ignored)
├── backups/              # Backup files (git-ignored)
├── logs/                 # Log files (git-ignored)
├── docs/                 # Documentation
├── templates/            # Zabbix templates
└── agents/               # Agent configurations
```

---

## 📝 Configuration

### Environment Variables (.env)
```bash
# Database Configuration
DB_NAME=zabbix
DB_USER=zabbix
DB_PASSWORD=your_secure_password_here
DB_HOST=postgres-server
DB_PORT=5432

# Zabbix Configuration
ZABBIX_SERVER_NAME=Zabbix Test Demo
ZABBIX_TIMEZONE=Asia/Bangkok

# Web Interface
WEB_PORT=8080
WEB_SSL_PORT=8443

# PostgreSQL
POSTGRES_PASSWORD=your_postgres_password_here
```

### Docker Compose Services
- `postgres-server` - PostgreSQL database
- `zabbix-server` - Zabbix server backend
- `zabbix-web` - Nginx + PHP frontend
- Network: `zabbix-net` (bridge network)

---

## 🛠️ Management Scripts

| Script | Purpose | Usage |
|--------|---------|-------|
| `start.sh` | Start all services | `./scripts/start.sh` |
| `stop.sh` | Stop all services | `./scripts/stop.sh` |
| `restart.sh` | Restart services | `./scripts/restart.sh` |
| `backup.sh` | Backup database | `./scripts/backup/backup.sh` |
| `restore.sh` | Restore database | `./scripts/backup/restore.sh` |
| `logs.sh` | View logs | `./scripts/monitoring/logs.sh` |
| `health-check.sh` | Check health | `./scripts/monitoring/health-check.sh` |

---

## 🔍 Monitoring Setup

### Add Host to Monitor
1. Login to Zabbix Web Interface
2. Navigate to: Configuration → Hosts
3. Click "Create host"
4. Configure:
   - **Host name:** your-host-name
   - **Groups:** Select appropriate group
   - **Interfaces:** Add Agent interface (port 10050)
5. Link templates and save

### Install Zabbix Agent on Target Host
```bash
# Ubuntu/Debian
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu24.04_all.deb
dpkg -i zabbix-release_7.0-1+ubuntu24.04_all.deb
apt update
apt install zabbix-agent

# Configure agent
nano /etc/zabbix/zabbix_agentd.conf
# Set: Server=your-zabbix-server-ip

# Start agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

---

## 🔐 Security Recommendations

### For Testing Environment
- ✅ Use strong passwords in `.env`
- ✅ Restrict access with firewall rules
- ✅ Keep Docker and images updated
- ✅ Regular backups

### For Production (ถ้าจะใช้งานจริง)
- 🔒 Enable HTTPS/SSL
- 🔒 Use secrets management
- 🔒 Implement network segmentation
- 🔒 Enable audit logging
- 🔒 Regular security updates
- 🔒 Monitoring access control

---

## 📊 Backup & Restore

### Manual Backup
```bash
# Backup database
./scripts/backup/backup.sh

# Backup will be saved in:
# backups/zabbix_backup_YYYYMMDD_HHMMSS.sql
```

### Restore from Backup
```bash
# List available backups
ls -la backups/

# Restore specific backup
./scripts/backup/restore.sh backups/zabbix_backup_20240101_120000.sql
```

### Automated Backup (Cron)
```bash
# Add to crontab
crontab -e

# Daily backup at 2 AM
0 2 * * * /path/to/scripts/backup/backup.sh
```

---

## 🐛 Troubleshooting

### Common Issues

#### 1. Cannot access web interface
```bash
# Check if services are running
docker-compose -f docker/docker-compose.yml ps

# Check firewall
ufw status
ufw allow 8080/tcp
```

#### 2. Database connection failed
```bash
# Check PostgreSQL logs
docker-compose -f docker/docker-compose.yml logs postgres-server

# Verify credentials in .env
cat .env | grep DB_
```

#### 3. High memory usage
```bash
# Check resource usage
docker stats

# Adjust PostgreSQL memory
# Edit: config/postgres/postgresql.conf
shared_buffers = 256MB
effective_cache_size = 1GB
```

### View Logs
```bash
# All services
docker-compose -f docker/docker-compose.yml logs -f

# Specific service
docker-compose -f docker/docker-compose.yml logs -f zabbix-server
```

---

## 📚 Documentation

- [Installation Guide](docs/INSTALL.md) - วิธีติดตั้งแบบละเอียด
- [Usage Guide](docs/USAGE.md) - วิธีใช้งาน Zabbix
- [Troubleshooting](docs/TROUBLESHOOTING.md) - แก้ปัญหาเบื้องต้น
- [Backup Procedures](docs/BACKUP.md) - วิธี Backup/Restore

### External Resources
- [Official Zabbix Documentation](https://www.zabbix.com/documentation/7.0/en/manual)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## 🤝 Contributing

ยินดีรับ Contributions! กรุณา:
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📧 Support & Contact

- **Author:** Your Name
- **Email:** your.email@rmutp.ac.th
- **Project Link:** [https://git-rmutp.ac.th/yourusername/Zabbix-Test-Demo](https://git-rmutp.ac.th/yourusername/Zabbix-Test-Demo)
- **Issues:** [Report Issues](https://git-rmutp.ac.th/yourusername/Zabbix-Test-Demo/issues)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Zabbix Team for the amazing monitoring solution
- Docker Team for containerization platform
- RMUTP for providing infrastructure
- Open source community

---

## 📈 Project Status

- [x] Initial setup
- [x] Docker Compose configuration
- [x] Basic documentation
- [ ] Security hardening
- [ ] Monitoring templates
- [ ] Auto-backup system
- [ ] Web UI customization
- [ ] Multi-host monitoring
- [ ] Alert notifications
- [ ] Performance tuning

---

**Last Updated:** October 2024
**Version:** 1.0.0-beta# Zabbix-Test-Demo
