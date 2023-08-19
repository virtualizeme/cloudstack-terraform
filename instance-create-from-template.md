# Tworzenie instancji VM w Apache CloudStack przy użyciu Terraform

## Wstęp

W tej instrukcji opisuję kroki, które pozwolą Ci stworzyć instancję wirtualnej maszyny (VM) w Apache CloudStack przy użyciu Terraform.

## Wymagania

- Zainstalowany [Terraform](https://www.terraform.io/downloads.html).
- Konto w Apache CloudStack.
- Dostęp do API w CloudStack.

## Krok po kroku

### 1. Konfiguracja Providera Terraform dla CloudStack

Tworzymy plik o nazwie `main.tf` i zaczynamy od konfiguracji providera:

\```hcl
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
\```

Zastąp wartości `URL_API_CLOUDSTACK`, `TWÓJ_KLUCZ_API` i `TWÓJ_SEKRETNY_KLUCZ` odpowiednimi danymi dostępu do Twojego Apache CloudStack.

### 2. Definiowanie instancji VM

W tym samym pliku `main.tf` dodajemy definicję VM:

\```hcl
resource "cloudstack_instance" "my_instance" {
  name           = "nazwa-instancji"
  display_name   = "Nazwa Wyświetlana"
  service_offering = "ID_OFEROWANEGO_SERWISU"
  template        = "ID_SZABLONU"
  zone            = "ID_STREFY"
  // ... inne parametry według potrzeb
}
\```

Zastąp powyższe wartości odpowiednimi dla Twojego środowiska CloudStack.

### 3. Inicjacja i planowanie zmian

Wykonaj następujące polecenia:

- `terraform init`: Polecenie to inicjuje katalog roboczy zawierający pliki konfiguracyjne Terraform. Pobiera też potrzebne wtyczki providera.

- `terraform plan`: To polecenie pozwala zobaczyć, jakie zmiany zostaną wprowadzone w Twoim środowisku przed ich rzeczywistym zastosowaniem. Jest to sposób na weryfikację i zaplanowanie zmian.

\```bash
terraform init
terraform plan
\```

### 4. Aplikacja zmian

Po sprawdzeniu planowanych zmian możemy je zastosować:

- `terraform apply`: Polecenie to stosuje zmiany niezbędne do osiągnięcia pożądanej konfiguracji zdefiniowanej w plikach konfiguracyjnych.

\```bash
terraform apply
\```

Po potwierdzeniu Terraform stworzy instancję wirtualną maszyny w Apache CloudStack.

## Dodatkowe informacje

Więcej informacji o providerze CloudStack dla Terraform znajdziesz [tutaj](https://registry.terraform.io/providers/cloudstack/cloudstack/latest/docs).
