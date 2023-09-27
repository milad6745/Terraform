دستور `terraform destroy` یکی از دستورات اساسی در ابزار مدیریت زیرساخت مانند Terraform است. این دستور برای حذف تمامی منابعی که توسط Terraform مدیریت می‌شوند از محیط استفاده می‌شود.

به طور دقیق‌تر، با اجرای دستور `terraform destroy`، Terraform تمامی منابع (مثلاً ماشین‌ها مجازی، شبکه‌ها، دیسک‌ها و ...) را که در فایل‌های کانفیگ ترمینولوژی آنها توصیف شده‌اند را حذف می‌کند. این کار شامل حذف پاک کردن منابع از زیرساخت مانند ابر، مراکز داده و یا سایر زیرساخت‌ها می‌شود.

دستور `terraform destroy` به عملیات حذف و از بین بردن منابعی می‌پردازد که توسط Terraform مدیریت می‌شوند، اما این کار تنها در صورتی امکان‌پذیر است که Terraform بتواند وضعیت فعلی منابع را بداند و بر اساس اطلاعات موجود در فایل‌های کانفیگ، اقدام به حذف آنها کند.


```shell
kubectl get pods -o wide --namespace example-namespace
NAME                                  READY   STATUS    RESTARTS   AGE   IP           NODE                 NOMINATED NODE   READINESS GATES
example-deployment-5d7fd759c8-8xtxt   1/1     Running   0          18h   10.244.0.6   kind-control-plane   <none>           <none>
example-deployment-5d7fd759c8-lcldv   1/1     Running   0          18h   10.244.0.5   kind-control-plane   <none>           <none>
example-deployment-5d7fd759c8-zzhmn   1/1     Running   0          18h   10.244.0.7   kind-control-plane   <none>           <none>


terraform destroy
Enter a value: yes

kubectl get pods -o wide --namespace example-namespace
No resources found in example-namespace namespace.
```
که در اینجا تمامی پاد های ایجاد شده توسط Terraform پاک میشود.
