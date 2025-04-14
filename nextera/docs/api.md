# Api-core
## Api Restful

### GENERAL

## Especificacions generals de l'aplicació

- API Restful per la gestió de les entitats de negoci (corporations, members, pharmacies, etc)
- Rutes semantiques (/entitats/entitat_id... /corporation/1)
- Métodes HTTP estàndards (GET, PUT, POST)
- Respostes en format JSON estàndard
- Codis de resposta HTTP estàndard (200, 404, 401, 403, 500)
- Autorizació d'endpoints via header tokens generats amb email + password

## Migracions

Les taules `personal_access_tokens` i `api_users` són necessàries pel funcionament del projecte Api-core. En bases de dades antigues o entorns locals és possible que s'hagin de crear aquestes taules mitjançant les següents migracions:

```bash
php artisan migrate --path=database/migrations/2020_10_14_000001_create_personal_access_tokens_table.php
```

```bash
php artisan migrate --path=database/migrations/2020_10_15_000000_create_api_users_table.php
```
## Migracions Tokens

Les taules `personal_access_tokens` i `api_users` són necessàries pel funcionament del projecte Api-core. En bases de dades antigues o entorns locals és possible que s'hagin de crear aquestes taules mitjançant les següents migracions:

```bash
php artisan migrate --path=database/migrations/2020_10_14_000001_create_personal_access_tokens_table.php
```

```sh
php artisan migrate --path=database/migrations/2020_10_15_000000_create_api_users_table.php

```

## Seeders

Executa el següent seeder per generar un usuari per la api-core amb les credencials estàndards:

```bash
php artisan module:seed Authorize
```


## Sincronitzar preus i stock de productes

### Procés extern

1. **Buscar les farmàcies que tenen la sincro activada**:

```sh
/pharmacies/search?corporation_products_syncronization=1
```

2. **Obtenir els productes de cada farmàcia:**

```sh
 /pharmacies/{pharmacyId}/products
```
3. **Actualitzar cada producte unitariament:**

```sh
/pharmacies/{pharmacyId}/products/{productId}
```

### Configuració de serveis i settings per activar la sincro

- `settings.pharmacy.corporation_products_syncronization = 1`
- `settings.pharmacy.connector.farmaofficego = 0` (Si té connector, NO POT ACTIVAR LA SINCRO)
- Un d'aquests serveis actius:
  - `Service::PROMOTIONS_ID(9)`
  - `Service::PARAPHARMACY_ID(10)`


### UTILITATS

### Generar PHPUnit report

1. **Accedir a la màquina virtual via `vagrant ssh`**.

2. **Entrar a la carpeta del projecte**:
```bash
   cd /path/to/project
```
3. **Executar:**

```sh
Executar vendor/bin/phpunit --coverage-html '/vagrant/api-core/storage/reports'
```

El proccés realitzà un analisis del codi de l'aplicació mitjançant els tests i genera una documentació navegable en format HTML. Es pot accedir al informe desde qualsevol navegador accedint a la carpeta de reports del projecte.

```sh
- [file://path/to/vestibule-referendum/api-core/storage/reports/index.html]

```
 Afegir alias 
```sh
- /vagrant/api-core/vendor/bin/phpunit --coverage-html '/vagrant/api-core/storage/reports' --configuration '/vagrant/api-core/phpunit.xml'

```





