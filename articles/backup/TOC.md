
# 概觀
## [何謂 Azure 備份？](backup-introduction-to-azure-backup.md)

# 開始使用
## [備份檔案和資料夾](backup-try-azure-backup-in-10-mins.md)
## [備份 Azure 虛擬機器](backup-azure-vms-first-look.md)
## [保護 Azure VM](backup-azure-vms-first-look-arm.md)

# 作法
## 使用 PowerShell
### [Azure 入口網站中的 Azure VM](backup-azure-vms-automation.md)
### [傳統入口網站中的 Azure VM](backup-azure-vms-classic-automation.md)
### [Azure 入口網站中的 DPM](backup-dpm-automation.md)
### [傳統入口網站中的 DPM](backup-dpm-automation-classic.md)
### [Azure 入口網站中的 Windows Server](backup-client-automation.md)
### [傳統入口網站中的 Windows Server](backup-client-automation-classic.md)

## Azure 備份伺服器
### [在 Azure 入口網站中準備 Azure 備份伺服器工作負載](backup-azure-microsoft-azure-backup.md)
### [在傳統入口網站中準備 Azure 備份伺服器工作負載](backup-azure-microsoft-azure-backup-classic.md)
### [使用 Azure 備份伺服器備份 VMware 伺服器](backup-azure-backup-server-vmware.md)
### [使用 Azure 備份伺服器備份 Exchange](backup-azure-exchange-mabs.md)
### [使用 Azure 備份伺服器備份 SharePoint 陣列](backup-azure-backup-sharepoint-mabs.md)
### [使用 Azure 備份伺服器備份 SQL](backup-azure-sql-mabs.md)

## Data Protection Manager
### [在 Azure 入口網站中準備 DPM 工作負載](backup-azure-dpm-introduction.md)
### [在傳統入口網站中準備 DPM 工作負載](backup-azure-dpm-introduction-classic.md)
### [使用 System Center DPM 備份 Exchange Server](backup-azure-backup-exchange-server.md)
### [將備份保存庫中的資料還原至替代的 DPM 伺服器](backup-azure-alternate-dpm-server.md)
### [使用 DPM 備份 SQL Server 工作負載](backup-azure-backup-sql.md)
### [使用 DPM 備份 SharePoint 伺服器陣列](backup-azure-backup-sharepoint.md)

## Azure VM
### [準備 Azure 虛擬機器](backup-azure-vms-prepare.md)
### [準備 Resource Manager 部署的虛擬機器](backup-azure-arm-vms-prepare.md)
### [規劃 VM 備份基礎結構](backup-azure-vms-introduction.md)
### [將 Azure 虛擬機器備份至備份保存庫](backup-azure-vms.md)
### [將 Azure 虛擬機器備份到復原服務保存庫](backup-azure-arm-vms.md)
### [備份及還原加密的虛擬機器](backup-azure-vms-encryption.md)
### [在傳統入口網站中管理及監視 Azure VM 備份](backup-azure-manage-vms-classic.md)
### [在 Azure 入口網站中管理 Azure VM 備份](backup-azure-manage-vms.md)
### [在 Azure 入口網站中監視 Azure VM 備份的警示](backup-azure-monitor-vms.md)
### [從 Azure VM 備份中復原檔案](backup-azure-restore-files-from-vm.md)
### [還原 Azure 中的虛擬機器](backup-azure-restore-vms.md)
### [還原 Azure 入口網站中 Resource Manager 部署的 VM](backup-azure-arm-restore-vms.md)
### [使用 Azure 備份還原已加密 VM 的 Key Vault 金鑰與密碼](backup-azure-restore-key-secret.md)

## Azure SQL Database
### [設定長期備份保留期](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [檢視復原服務保存庫中的備份](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [從長期備份保留期還原](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [刪除長期的 Azure SQL 備份資料](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Windows 檔案和資料夾
### [使用傳統部署模型的 Windows Server](backup-configure-vault-classic.md)
### [使用 Resource Manager 部署模型的 Windows Server](backup-configure-vault.md)
### [使用傳統部署模型管理備份保存庫](backup-azure-manage-windows-server-classic.md)
### [監視及管理復原服務保存庫](backup-azure-manage-windows-server.md)
### [使用 Resource Manager 部署模型將檔案復原到 Windows Server](backup-azure-restore-windows-server.md)
### [使用傳統部署模型將檔案復原到 Windows Server](backup-azure-restore-windows-server-classic.md)

## [常見問題集](backup-azure-backup-faq.md)

## 疑難排解
### [Azure 入口網站中的 Azure VM 備份問題](backup-azure-vms-troubleshoot.md)
### [傳統入口網站中的 Azure VM 備份問題](backup-azure-vms-troubleshoot-classic.md)
### [Azure 備份伺服器](backup-azure-mabs-troubleshoot.md)
### [Azure VM 備份失敗︰無法與 VM 代理程式通訊來取得快照集狀態 - 快照集 VM 子工作已逾時](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Azure 備份中的檔案和資料夾備份速度緩慢](backup-azure-troubleshoot-slow-backup-performance-issue.md)

# 概念
## [復原服務保存庫概觀](backup-azure-recovery-services-vault-overview.md)
## [將備份保存庫升級為復原服務保存庫](backup-azure-upgrade-backup-to-recovery-services.md)
## [刪除 Azure 備份保存庫](backup-azure-delete-vault.md)
## [角色型存取控制](backup-rbac-rs-vault.md)
## [混合式備份的安全性](backup-azure-security-feature.md)
## [設定離線備份](backup-azure-backup-import-export.md)
## [取代您的磁帶程式庫](backup-azure-backup-cloud-as-tape.md)
## [Linux VM 的應用程式一致備份](backup-azure-linux-app-consistent.md)

# 參考
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# 資源
## [價格](https://azure.microsoft.com/pricing/details/backup/)
## [MSDN 論壇](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [影片](https://azure.microsoft.com/documentation/videos/index/?services=backup)
## [服務更新](https://azure.microsoft.com/updates/?product=backup)
