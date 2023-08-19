# Tworzenie instancji VM w Apache CloudStack przy użyciu Terraform i pliku `variables.tf`

## Wstęp

W tej instrukcji opisuję, jak korzystać z pliku `variables.tf` w projekcie Terraform, aby parametryzować tworzenie instancji w Apache CloudStack.

## Wymagania

- Zainstalowany [Terraform](https://www.terraform.io/downloads.html).
- Konto w Apache CloudStack.
- Dostęp do API w CloudStack.

## Krok po kroku

### 1. Konfiguracja Providera Terraform dla CloudStack

Tworzymy plik o nazwie `main.tf`:

```hcl
terraform {
  required_providers {
    cloudstack = {
      source  = "cloudstack/cloudstack"
      version = "0.4.0"
    }
  }
}

provider "cloudstack" {
  api_url    = var.api_url
  api_key    = var.api_key
  secret_key = var.secret_key
}
```

### 2. Definicja zmiennych w `variables.tf`

Tworzymy plik `variables.tf`, w którym zdefiniujemy wymagane zmienne:

```hcl
variable "api_url" {
  description = "URL API dla CloudStack"
  type        = string
}

variable "api_key" {
  description = "Klucz API dla CloudStack"
  type        = string
}

variable "secret_key" {
  description = "Sekretny klucz dla CloudStack"
  type        = string
}

variable "service_offering_id" {
  description = "ID oferowanego serwisu dla instancji VM"
  type        = string
}

variable "template_id" {
  description = "ID szablonu dla instancji VM"
  type        = string
}

variable "zone_id" {
  description = "ID strefy dla instancji VM"
  type        = string
}
```

### 3. Utworzenie instancji VM przy użyciu zdefiniowanych zmiennych

W pliku `main.tf`, dodajemy następujący kod:

```hcl
resource "cloudstack_instance" "my_instance" {
  name             = "instancja-example"
  service_offering = var.service_offering_id
  template         = var.template_id
  zone             = var.zone_id
  // ... inne parametry według potrzeb
}
```

### 4. Utworzenie pliku `terraform.tfvars`

Aby dostarczyć wartości dla zdefiniowanych zmiennych, tworzymy plik `terraform.tfvars`:

```hcl
api_url            = "URL_API_CLOUDSTACK"
api_key            = "TWÓJ_KLUCZ_API"
secret_key         = "TWÓJ_SEKRETNY_KLUCZ"
service_offering_id = "ID_OFEROWANEGO_SERWISU"
template_id         = "ID_SZABLONU"
zone_id             = "ID_STREFY"
```

### 5. Inicjacja, planowanie i aplikacja zmian

Zainicjuj katalog roboczy, zaplanuj zmiany i zastosuj je:

```bash
terraform init
terraform plan
terraform apply
```

Terraform użyje wartości z pliku `terraform.tfvars` do stworzenia instancji VM w Apache CloudStack.

## Dodatkowe informacje

Więcej informacji o providerze CloudStack dla Terraform znajdziesz [tutaj](https://registry.terraform.io/providers/cloudstack/cloudstack/latest
