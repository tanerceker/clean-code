# Clean Code

Temiz kod her zaman önemseyen biri tarafından yazılmış gibi görünür.

Daha iyi hale getirmek için yapabileceğiniz bariz bir şey yoktur.

— Michael Feathers

---

**(KISS)** Keep It Simple, Stupid — Basit Tut, Aptal

**(DRY)** Don’t Repeat Yourself — Kendini Tekrar Etme

---

<br/>

Name Casing — Ad Muhafazası

```tsx
// camelCase

let firstName = "Taner";
let lastName = "Çeker";
```

<br/>

```tsx
// PascalCase

class UserAuth {}
class UserSettings {}
```

<br/>

```tsx
// snake_case

let user = { first_name: "Taner", last_name: "Çeker" };

// SNAKE_CASE

const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; // 86400000
```

<br/>

```tsx
// kebab-case

let user = { "first-name": "Taner", "last-name": "Çeker" };
```

---

<br/>

Meaningful — Anlamlı

```tsx
const data = { id: 1, name: "Taner" }   ❌

const user = { id: 1, name: "Taner" }   ✅

// Bağlam'a (Context) bağlı olarak anlamlı ad'lar kullanılmalı.
// Örn. para transfer yapan birini işaret ediyorsak;
// "customer" daha anlamlıdır

const customer = { id: 1, name: "Taner" }   ✅
```

<br/>

Pronounceable — Telaffuz edilebilir

```tsx
const yyyymmdd = moment().format("YYYY/MM/DD")   ❌

const currentData = moment().format("YYYY/MM/DD")   ✅
```

<br/>

Detailed — Detaylı, Açıklayıcı

```tsx
const days = 12   ❌

const time = 3756   ❌

const daysSinceEventCreation = 12   ✅

const timeSinceLastCheck = 3756   ✅
```

<br/>

Booleans — Mantıksallar

```tsx
// is, are, should, has, etc ...

let loading = true   ❌

let productsInCart = false   ❌

let isLoading = true   ✅

let user.hasProductsInCart = false   ✅
```

---

<br/>

No redundant words — Gereksiz kelimeler yok

```tsx
// data, info, record, list, etc...

const userData = {}   ❌

const productList = []   ❌

const taskInfo = {}   ❌

const user = {}   ✅

const products = []   ✅

const task = {}   ✅
```

---

<br/>

Her zaman açık olun — Always be explicit

```tsx
const daysOfTheWeek = ["Monday", "Tuesday", "Wednesday", "Thursday",
"Friday", "Saturday", "Sunday"]

daysOfTheWeek.forEach(d => {   ❌
  something(d)
  /*
  ...
  ...
  ...
  */
  somethingElse(d)
})

daysOfTheWeek.forEach(dayOfTheWeek => {   ✅
  something(dayOfTheWeek)
  /*
  ...
  ...
  ...
  */
  somethingElse(dayOfTheWeek)
})
```

---

<br/>

Sabitlerin adlandırılması — Naming constants

```tsx
const MAX_NUMBER_OF_RETRY = 3;

const userName = "Taner";

const maxNumberOfExecutions = 3;

for (let i = 0; i < maxNumberOfExecutions; i++) {
  sendMessageByTwitter(userName);
}
```

---

<br/>

Fonksiyon adlandırması — Function naming

```tsx
// Use verbs - Fiilleri kullanın

const saveNewPassword = () => {};
const fetchArticles = () => {};
const isEven = () => {};

// Consistent - Tutarlılık

const getComments = () => {};
const fetchArticles = () => {};
const retrieveCategory = () => {};

// Details - Detaylılık

const saveNewPassword = () => {};
const fetchArticlesByCategoryName = () => {};
```

---

<br/>

Fonksiyonları kısa tutun — Keep short functions

```tsx
// 1. Fonksiyonların ilk kuralı küçük olmaları gerektiğidir.
// 2. İkinci kural ise daha da küçük olmaları gerektiğidir.


// 1. Sadece tek bir şey yapmalı.

async function login(username, password) {   ❌
  const accessToken = await database.login(username,password)
  if (!accessToken) {
    throw new Error('Wrong credentials')
  }

  localStorage.setItem('accessToken', accessToken)

  redirectTo("/")
  notifyUser("success", e.message)
}

async function login(username, password) {   ✅
  const accessToken = await database.login(username,password)
  if (!accessToken) {
    throw new Error('Wrong credentials')
  }

  localStorage.setItem('accessToken', accessToken)
}


// 2. Diğer fonksiyonların ayıklanması (extract)

function submitLogin(username, password) {   ❌
  try {
    if (username.length < 3) {
      throw new Error('Username must be at least 3 characters long')
    }
    if (password.length < 3) {
      throw new Error('Password must be at least 3 characters long')
    }

    await login(username, password)

    redirectTo("/")
    notifyUser("success", e.message)

  } catch (e) {
    console.error(e)
    notifyUser("error", e.message)
  }
}

function submitLogin(username, password) {   ✅
  try {
    validateLoginFields(username, password)

    await login(username, password)

    redirectTo("/")
    notifyUser("success", e.message)

  } catch (e) {
    console.error(e)
    notifyUser("error", e.message)
  }
}

function validateLoginFields(username, password) {
  if (username.length < 3) {
    throw new Error('Username must be at least 3 characters long')
  }
  if (password.length < 3) {
    throw new Error('Password must be at least 3 characters long')
  }
}
```

---

<br/>

Parametre yönetimi — Parameters management

```tsx
// Maksimum iki parametre kuralı

function updateUserAdress(country, city, postalCode, street) {   ❌

}

updateUserAdress("France", "Paris", "75000", "rue de la Republique")

function updateUserAdress(country, city) {   ✅

}

updateUserAdress("France", "Paris")


// Nesne kullanarak daha fazla parametre kullanılabilir

function updateUserAdress({country, city, postalCode, street}) {   ✅

}

updateUserAdress({
  country: "France",
  city: "Paris",
  postalCode: "75000",
  street: "rue de la Republique",
})
```

---

<br/>

Varsayılan değerler — Default values

```tsx
function sleep(durationInMilliseconds) {   ❌
  durationInMilliseconds = durationInMilliseconds ?? 10000
  return new Promise(resolve => setTimeout(resolve, durationInMilliseconds))
}

function sleep(durationInMilliseconds = 1000) {   ✅
  return new Promise(resolve => setTimeout(resolve, durationInMilliseconds))
}

function createTask(config) {   ❌
  config.title = config.title ?? "Untitled task"
  config.category = config.category ?? "Main"
  config.isActive = config.isActive ?? true
  // ...
}

// Varsayılan nesneler için Object.assign()

const defaultConfig = {
  title: 'Untitled task',
  category: 'Main',
  isActive: true
}

function createTask(rawConfig) {   ✅
  const config = Object.assign(defaultConfig, rawConfig)
  // ...
}

createTask({
  title: 'Recording',
  category: 'DevTheory'
})
```

---

<br/>

Bayrak parametreleri — Flag parameters

```tsx
// Bayrak (Flag) parametrelerini kullanmayın

function updateUser(isPremium) {   ❌
  if (isPremium) {
    /* ... */
  } else {
    /* ... */
  }
}

if (isPremium) {   ✅
  updatePremiumUser()
} else {
  updateFreeUser()
}

function updatePremiumUser() {

}

function updateFreeUser() {

}

// Tek bir işlem hariç

function updateUser(isPremium) {   ✅
  something()
  /*
  	...
  */
  if (isPremium) {
    sendUpdateConfirmationOnTelegram()
  } else {
    sendUpdateConfirmationOnByMail()
  }
}

```

---

<br/>

Kaçınılması gereken şeyler — Things to avoid

```tsx
// Global / üst kapsam (upper-scope) değişkenleri kullanmaktan kaçının

const code = "let x = 5;let y = 10;let z = x + y;"

const lines = splitCodeIntoLines()

console.log(lines) // [ 'let x = 5', 'let y = 10', 'let z = z + y' ]

function splitCodeIntoLines() {   ❌
  return code.split(";")
}

// Referansların değerini değiştirmekten kaçının

function addItemToCart(cart, item) {    ❌
  cart.push({ item, date: Date.now() })
}

function addItemToCart(cart, item) {   ✅
  return [...cart, { item, date: Date.now() }]
}
```

---

<br/>

Sınıflar veya Fonksiyonlar — Classes or Functions

```tsx
// Basit sınıflar yerine basit fonksiyonları tercih edin
```

---

<br/>

Bileşim veya Kalıtım — Composition or Inheritance

Kalıtım (Inheritance)
→ Örn. Kuş bir hayvandır. — Insan bir memelidir. (is-a)

Bileşim (Composition)
→ Örn. Arabanın bir motoru var. — Araba bir motora sahiptir. (has-a)

```tsx
// Kalıtım (Inheritance) yerine Bileşimi (Composition) tercih edin
// ilişki türleri: is-a vs has-a

// Kalıtım (Inheritance) (is-a)

class Market {
  constructor(symbol) {
    this.symbol = symbol;
  }

  /* ... */
}

class MarketOrder extends Market {
  constructor(symbol, price, quantity) {
    super(symbol);
    this.price = price;
    this.quantity = quantity;
  }

  /* ... */
}

// Bileşim (Composition) (has-a)

class Market {
  constructor(symbol) {
    this.symbol = symbol;
  }

  /* ... */
}

class Order {
  constructor(price, quantity) {
    this.price = price;
    this.quantity = quantity;
  }

  setMarket(symbol) {
    this.market = new Market(symbol);
  }

  /* ... */
}
```

---

<br/>

Hataları görmezden gelmeyin — Don't ignore errors

```tsx
// Hatalar görmezden gelinemez.
// Dış (External) API (Kontrolü size ait olamayan) için
// hata kontrolleri kullanılmalı.

let isLoading = true;

try {
  await fetchArticles();
} catch (e) {
  notifyUser("Failed to fetch articles");
  reportError(e);
} finally {
  isLoading = false;
}
```

---

<br/>

Fonksiyonlar için JSdoc kullanılabilir — Use JSdoc for functions

```tsx
/**
 * Validates login fields
 *
 * @param {string} email
 * @param {string} password
 * @returns {boolean} The boolean returned
 * */

function validateLoginFields(email, password) {
  if (email.length > 0) {
    throw new ValidationError("Email is required");
  } else if (password.length > 0) {
    throw new ValidationError("Password is required");
  }
  return true;
}
```

<br/>
<br/>
