# Tworzenie trzech instancji VM w Apache CloudStack przy użyciu Terraform i `count`

## Wstęp

W tej instrukcji opisuję kroki, które pozwolą Ci stworzyć trzy instancje wirtualnej maszyny (VM) w Apache CloudStack przy użyciu Terraform, bazując na jednym szablonie i wykorzystując funkcję `count`.

## Wymagania

- Zainstalowany [Terraform](https://www.terraform.io/downloads.html).
- Konto w Apache CloudStack.
- Dostęp do API w CloudStack.

## Krok po kroku

### 1. Konfiguracja Providera Terraform dla CloudStack

Tworzymy plik o nazwie `main.tf` i zaczynamy od konfiguracji providera:

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

### 2. Definiowanie trzech instancji VM z jednego szablonu

W tym samym pliku `main.tf`, użyjemy `count` aby stworzyć trzy instancje bazujące na jednym szablonie:

```hcl
resource "cloudstack_instance" "my_instances" {
  count          = 3
  name           = "instancja-${count.index}"
  display_name   = "Instancja-${count.index} Wyświetlana"
  service_offering = "ID_OFEROWANEGO_SERWISU"
  template        = "ID_SZABLONU"
  zone            = "ID_STREFY"
  // ... inne parametry według potrzeb
}
```

Zmienna `count.index` automatycznie numeruje instancje od 0 do 2 (dla `count = 3`), co pozwala na stworzenie unikalnych nazw dla każdej z instancji.

### 3. Inicjacja i planowanie zmian

Zainicjuj katalog roboczy i zaplanuj zmiany:

```bash
terraform init
terraform plan
```

### 4. Aplikacja zmian

Po sprawdzeniu planowanych zmian możemy je zastosować:

```bash
terraform apply
```

Terraform stworzy trzy instancje wirtualnych maszyn w Apache CloudStack.

## Dodatkowe informacje

Więcej informacji o providerze CloudStack dla Terraform znajdziesz [tutaj](https://registry.terraform.io/providers/cloudstack/cloudstack/latest/docs).
