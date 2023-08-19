# Tworzenie trzech sieci i trzech instancji VM w Apache CloudStack przy użyciu Terraform

## Wstęp

W tej instrukcji przedstawiam, jak stworzyć trzy sieci oraz trzy instancje wirtualne, z których każda jest przypisana do jednej z tych sieci w Apache CloudStack przy użyciu Terraform.

## Wymagania

- Zainstalowany [Terraform](https://www.terraform.io/downloads.html).
- Konto w Apache CloudStack.
- Dostęp do API w CloudStack.

## Krok po kroku

### 1. Konfiguracja Providera Terraform dla CloudStack

W pliku `main.tf` dodajemy:

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
  api_url    = "URL_API_CLOUDSTACK"
  api_key    = "TWÓJ_KLUCZ_API"
  secret_key = "TWÓJ_SEKRETNY_KLUCZ"
}
```

### 2. Definiowanie trzech sieci

Dalej w pliku `main.tf`:

```hcl
resource "cloudstack_network" "my_network" {
  count = 3
  name = "sieć-${count.index}"
  display_text = "Sieć-${count.index}"
  network_offering = "ID_OFERTY_SIECI"
  zone = "ID_STREFY"
  // ... inne parametry według potrzeb
}
```

### 3. Tworzenie trzech instancji VM i przypisywanie ich do sieci

Kontynuując w `main.tf`:

```hcl
resource "cloudstack_instance" "my_instance" {
  count = 3
  name = "instancja-${count.index}"
  display_name = "Instancja-${count.index}"
  service_offering = "ID_OFEROWANEGO_SERWISU"
  template = "ID_SZABLONU"
  zone = "ID_STREFY"
  network_id = cloudstack_network.my_network[count.index].id
  // ... inne parametry według potrzeb
}
```

### 4. Inicjacja, planowanie i aplikacja zmian

Zainicjuj katalog roboczy, zaplanuj zmiany i zastosuj je:

```bash
terraform init
terraform plan
terraform apply
```

Terraform stworzy trzy sieci oraz trzy instancje VM, z których każda jest przypisana do jednej z tych sieci w Apache CloudStack.

## Dodatkowe informacje

Więcej informacji o providerze CloudStack dla Terraform znajdziesz [tutaj](https://registry.terraform.io/providers/cloudstack/cloudstack/latest/docs).
