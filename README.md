# Exercise 6.2 — ALB with Listener and Target Group

Curso: Optimizaciones y Desempeño — Cloud Deployment Automation

## Objetivo

Crear una capa de Application Load Balancer para una instancia EC2 existente, utilizando data sources para descubrir la infraestructura base sin hardcodear IDs.

## Arquitectura

```text
Internet
↓
Application Load Balancer
↓
Listener HTTP :80
↓
Target Group
↓
EC2 instance mediastream-api :80
```
---

## Infraestructura Creada
+ ALB Security Group
+ Application Load Balancer
+ Target Group
+ HTTP Listener
+ Target Group Attachment

---

## Data sources utilizados
+ aws_vpc
+ aws_subnets
+ aws_instance

---

## Validación HTTP
```bash
curl http://mediastream-alb-1055834289.us-east-1.elb.amazonaws.com
```
El ALB respondió correctamente con el servidor HTTP Python de la instancia EC2.

![Respuesta](/evidence/alb_response.PNG)

---
## Comandos utilizados
```bash
cd setup
terraform init
terraform apply -auto-approve

cd ..
terraform init
terraform validate
terraform apply -var-file=terraform.tfvars
```
para destruir
```bash
terraform destroy -var-file=terraform.tfvars

cd setup
terraform destroy -auto-approve
```
---

## Evidence
### Terraform state list
```text
data.aws_instance.api
data.aws_subnets.public
data.aws_vpc.main
aws_lb.main
aws_lb_listener.http
aws_lb_target_group.api
aws_lb_target_group_attachment.api
aws_security_group.alb_sg

```
### Terraform outputs
```text
alb_arn = "arn:aws:elasticloadbalancing:us-east-1:577133972654:loadbalancer/app/mediastream-alb/74f219256054b5a9"
alb_dns_name = "mediastream-alb-1055834289.us-east-1.elb.amazonaws.com"
target_group_arn = "arn:aws:elasticloadbalancing:us-east-1:577133972654:targetgroup/mediastream-api-tg/83e57f11862305ad"
```

### AWS Console
![AWS Console ](/evidence/aws_console.PNG)

---
## Conceptos aplicados
+ Application Load Balancer
+ Target Group
+ Listener
+ Target registration
+ Data sources
+ Health checks
+ Internet-facing ALB
+ Public subnets
+ Security Groups

---

## Autor
Sergio Geovany García Smith

Carnet 25008130