<p style="text-align: center;"><img alt="Nowak Digital Solutions logo" src="https://raw.githubusercontent.com/nowak-digital-solutions/kalkulator-ubezpieczeniowy/main/images/nds-logo.png" width="450"></p>

# Kalkulator ubezpieczeń - Dokumentacja

Dokument ten opisuje funkcje oraz instrukcję instalacji formularza ubezpieczeń.

## 1. Instalacja

Operacja jest bardzo prosta i wymaga dodania do kodu HTML strony kilku tagów.

``` 
<formularz-do-leadow agent-id="AAAA"  agent-meta="BBBB"></formularz-do-leadow>
<script src="https://public.kalkulator.nowakdigitalsolutions.com/formularz-do-leadow.js"></script>
```

Klucz agent-id oraz agent-meta są dostarczane po po podpisaniu umowy.

## Pola konfiguracji 
Formularz przyjmuje kilka parametrów które sterują wyglądem oraz zachowaniem formularza

Aby ustawić pole należy przekazać atrybut do formularza wraz z wartością.

### Dostępne atrybuty
#### zrodlo
```html
<formularz-do-leadow zrodlo="nazwa">
```
Źródło formularza dodaje komentarz do kalkulacji, który określa skąd przyszła kalkulacja. Jeśli posiadamy formularz zainstalowany na kilku podstronach lub w innej domenie, możemy podać nazwę, która określi kalkulację.

#### wylacz-domyslne-style
```html
<formularz-do-leadow wylacz-domyslne-style="true">
```
Domyślna wartość: `false`

Jeśli chcemy aby formularz korzystał tylko ze styli dostarczonych przez stronę. W tym wypadku dodawany jest minimalny zestaw styli, który tylko definiuje loader.

#### wyswietl-dialog
```html
<formularz-do-leadow wyswietl-dialog="true">
```
Domyślna wartość: `true`
Jeśli formularz został skonfigurowany tak by nie liczyć składek, atrybut ten określa czy zostanie wyświetlony uzytkownikowi dialog informujący i wysłaniu zapytania. Jeśli agencja chce przekierować użytkowika na inną stronę, z podsumowaniem wtedy należy ustawić atrybut na `false` i gdy przyjdzie zdarzenie `formSubmitted` zrobić przekierowanie.

#### tekst-przycisku-wyslij
```html
<formularz-do-leadow tekst-przycisku-wyslij="Oblicz składki">
```
Domyślna wartość: `Wyślij`

Ustawia tekst na przycisku.

#### przeladuj-po-wyslaniu
```html
<formularz-do-leadow przeladuj-po-wyslaniu="true">
```
Domyślna wartość: `false`
Jeśli formularz ma przeładować stronę po wysłaniu. Lepiej skorzystać z eventu `formSubmitted` i obsłużyć tę akcję we własnym zakresie.

#### pokaz-loader
```html
<formularz-do-leadow pokaz-loader="false">
```
Domyślna wartość: `true`
Jeśli chcemy użyć własnego loadera by wygląd był spójny ze stroną, możemy schować domyślny loader. Należy wtedy obsłuzyć dwa zdarzenia (Patrz sekcja na końcu dokumentacji)
* `loaderHidden`
* `loaderShowed`

## Konstrukcja formularza

Formularz składa się z dwóch sekcji:

* Formularz
* Strona ze składkami

### Sekcja Formularza

Po wejściu na stronę klient widzi pierwsze elementy formularza. Na poczatku klient musi wybrać dane pojazdu. Formularz

#### Struktura wierszy

Wiersze skłądają się zawsze z tych samych elemnentów:

* Etykieta pola tag `<label>`
* wprowadzenie danych tag `<fieldset>`
* Komunikat błędu pola -tag `<legend>`
* Niektóre pola zawierają ikonę pomocy, po kliknięciu na którą pojawia się podpowiedź gdzie znaleźć informację o danym
  polu.

Każdy wiersz posiada swój unikalny identyfikator

### Kod formularza

Poniżej znajduje się kod kompletnego wiersza z kafelkami do wyboru rodzaju pojazdu:

```html
<div class="form-row">
    <app-question>
        <div class="form-group invalid ng-untouched ng-pristine ng-invalid" id="vehicleTypeCode">
            <label for="vehicleTypeCode">Rodzaj pojazdu:</label>
            <div>
                <fieldset class="options errorMessage">
                    <input type="hidden" id="vehicleTypeCode" value="" class="ng-untouched ng-pristine ng-invalid">

                    <button type="button" class="ml-1 mt-1 form-tile-button">Samochód osobowy</button>
                    <button type="button" class="ml-1 mt-1 form-tile-button">Motocykl</button>
                    <button type="button" class="ml-1 mt-1 form-tile-button">Samochód dostawczy</button>
                    <span class="help"></span>
                    <legend>Należy wybrać jedną opcję</legend>
                </fieldset>
            </div>
        </div>
    </app-question>
</div>
```

## Style

Formularz przychodzi z gotowym zestawem styli. Style w zależności od szablonu strony, mogą wymagać dopasowania. Zadaniem
administratora strony jest dostosowanie wyglądu formularza do wyglądu strony, na której został zainstalowany.

Formularz opakowany jest w główny tag `formularz-do-leadow`

Istnieje możliwość wyłączenia styli formularza i dostosowania go od zera do styli strony. Aby to zrobić należy dodać
atrybut `wylacz-domyslne-style="true"` do głównego taga formularza.

### Uwaga

Style formularza w większości przypadków dopasowują się dobrze do strony. Należy zwrócić uwagę/zmodyfikować na kilka
elementów:

* kolory oraz ramki przycisków
* kolory oraz ramki pól input
* Szerokość taga `<label>` w wierszu
* Szerokość tagu '<div>' w wierszu
* Prawidłowe wyświetlanie checkboxów
* Styl ramek z wynikami

### Sekcja Formularza

Sekcja formularza obudowana jest w tag `<div class="lead-form">`. Jeden poziom niżej znajduje się tag `<form>`. Tag ten
posiada klasy css w zależności od stanu formularza.

#### Klasy walidacji

Tag ten może zawierać kilka róznych klas w zależności od stanu formularza:

* `ng-untouched` Formularz nie został 'dotknięty' przez użytkownika
* `ng-touched` Formularz został 'dotknięty' przez użytkownika
* `ng-pristine` Formularz nie został 'zmodyfikowany' przez użytkownika
* `ng-dirty` Formularz został 'zmodyfikowany' przez użytkownika
* `ng-invalid` Formularz zawiera pola, które nie przeszły walidacji (są nieprawidłowe)
* `ng-valid` Formularz przeszedł walidację.

#### Wiersze formularza

Formularz posiada kilka róznych typów wierszy. W zależności od typu struktura htmla może sie różnić. Poniżej znajdują
się przykłady poszczególnych typów.

* Przyciski wyboru
* Dropdown
* Pola Input
* Data - pole input wraz z przyciskami wyboru daty.
* Pola zaznaczania (checkboxy)

Wiersz formularza obudowany jest w tag `<div class="form-row">` oraz głębiej w tag `<div class="form-group">`. W tym
tagu pojawiają się te same klasy walidacji jak w głównym tagu formularza (patrz wyżej sekcja 'Klasy walidacji'). głębiej
znadują się dwa tagi:

* label - etykieta pola
* div - zawartość pola

### Sekcja ofert

Kiedy formularz został prawidłowo wysłany, na stronie zaczną pojawiać się kafelki z ofertami. Kafle te zajmują całą
szerokość strony. Kolejne kafle doładowywane są w miare jak informacje zaczynają wracać od TU. W trakcie przeliaczania
pojawia pod kaflami widoczny jest loader.

```html

<div class="offer-tile">
  <h2>4 817.18 zł</h2>
    <p class="description">OC, Assistance: Podstawowy, NNW: 5 000 zł, AC: Kosztorys, Udział własny w szkodzie: 500 zł,
        Amortyzacja części zamiennych</p>
  <span>dba5ea62-b27a-4e1f-ad5e-2d6814910b12</span>
</div>
```
W chwili obecnej nie jest wyświetlane logo TU, z powodu braku zgody wszystkich TU na wyświetlanie oferty na stronach.

## Komunikacja strony z formularzem - zdarzenia

Formularz podczas interakcji z użytkownikiem może wysyłać zdarzenia, które strona może odpowiednio przetwarzać.
Zdarzenia są wysyłane za pomocą metody JS która może zawierać dane.

Poniżej zdarzenie któ®e formularz wysyła gdy użytkownik wyśle zapytanie o przeliczenie składek:

```html

<script>
    window.subscribeToLeadFormEvent('formSubmitted', (payload) => {
        console.log('wyslano formularz', payload)
    })
</script>
```

### Lista zdarzeń

#### 'formSubmitted'

Formularz został wysłany.
Payload:

```ts

export interface SubmitPublicEvent {

    vehicleProductionYear: number;
    vehicleMakeId: number;
    vehicleMakeName: string;
    vehicleFuelTypeCode: string;
    vehicleEngineCapacity: number;
    vehicleBodyTypeCode: string;
    vehicleTypeCode: string;
    vehicleMileage: number;
    vehicleRegistrationNumber?: string;
    vehicleVinNumber: string;
    vehicleFirstRegistrationDate?: string;
    vehicleAcquisitionYear: number;
    vehicleSeatNumber: number;
    vehicleIsSteeringWheelOnTheLeft?: boolean;

    vehicleEurotaxModelId: number,
    vehicleEurotaxVersionId: number,
    vehicleEurotaxModelName: string,
    vehicleEurotaxVersionName: string,

    "vehicleInfoexpertModelId": number,
    "vehicleInfoexpertVersionId": number,
    "vehicleInfoexpertModelName": string,
    "vehicleInfoexpertVersionName": string,

    insuranceStartDate: string;
    contactEmail: string;
    contactPhoneNumber: string;
    owner?: PublicOwner;
    source: string;
    comment?: string;
    coowners?: PublicOwner[];
}

export interface PublicOwner {
    firstName?: string;
    lastName?: string;
    pesel?: string;
    postalCode?: string;
    city?: string;
    street?: string;
    houseNumber?: string;
    flatNumber?: string;
    documentIssueDate?: Moment | string;
    birthDate?: Moment | string;
}

```

#### 'calculationFinished'

Zakończno kalkulowanie składek.

Payload:

```ts

export class CalculationOffer {

  name: string | undefined //opis składki, który przychodzi od zakładu.
  price: string | undefined 
  insurerName: string | undefined
  offerLog: string | undefined //unikalny klucz ofert z QMT - 
}
```

#### 'formConfirmationRequired'

Zdarzenie wysyłane jeśli formularz wymaga potwierdzenia przez klienta adresu mailowego poprzez kliknięcie na link w
wysłanym mailu.

#### 'loaderHidden'

Schowano loader - jeśli domyślny loader jest wyłaczony na stronie. Istnieje możliwość zastąpienia domyślnego loadera
tym, który jest wykorzystany na stronie.

#### 'loaderShowed'

Pokazano loader


# Licencja
Wyłącznie do użytku przez klientów posiadających umowę na integrację formularza. Własność Nowak Digital Solutions Sp. z o.o.
