# install teraform


برای نصب Terraform در اوبونتو، می‌توانید از مدیر بسته‌ها (package manager) apt استفاده کنید. دقت کنید که باید دسترسی مدیر سیستم یا حساب کاربر با دسترسی sudo داشته باشید.

مراحل زیر را دنبال کنید:

1. **باز کردن ترمینال**:
   ابتدا ترمینال را اجرا کنید. می‌توانید این کار را از طریق فشار دادن `Ctrl + Alt + T` یا جستجوی "Terminal" در منوی برنامه‌ها انجام دهید.

2. **افزودن مخزن HashiCorp**:
   برای نصب Terraform، می‌توانید مخزن (repository) HashiCorp را به لیست مخازن سیستم خود اضافه کنید. این کار با اجرای دستورات زیر انجام می‌شود:

   ```bash
   sudo apt-get update
   sudo apt-get install -y gnupg software-properties-common
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 51716619E084DAB9
   sudo add-apt-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   ```

3. **نصب Terraform**:
   حالا که مخزن HashiCorp به لیست مخازن افزوده شده است، می‌توانید Terraform را نصب کنید:

   ```bash
   sudo apt-get update
   sudo apt-get install terraform
   ```

4. **بررسی نصب**:
   بعد از اجرای دستورات فوق، Terraform باید بر روی سیستم شما نصب شده باشد. برای بررسی نصب، می‌توانید از دستور زیر استفاده کنید:

   ```bash
   terraform --version
   ```

   این دستور اطلاعات نسخه‌ی نصب شده از Terraform را نشان می‌دهد.

حالا شما Terraform را بر روی سیستم اوبونتو خود نصب کرده‌اید و می‌توانید از آن برای مدیریت زیرساخت‌های خود استفاده کنید.

## install from zip

```
https://releases.hashicorp.com/terraform/1.5.7/terraform_1.5.7_linux_amd64.zip
unzip terraform_1.5.7_linux_amd64.zip
mv terrraform /usr/local/bin
chmod +x terraform
```
