## Bulding Large Scale Web Apps

### Internationalization

As the internet continues to connect people all over the world, it becomes 
increasingly important to design and develop web applications that cater 
to users from various regions and locales. This process is known 
as internationalization, and it ensures that our applications are 
accessible, functional, and culturally appropriate for users all over the 
world.

Good internationalization practices lead to:

- An increased user base
- An enhanced user experience
- Compliance with regulations

#### Separate text and content from code

One of the most important things we can do to facilitate easy translation 
and adaptation of text in a large JavaScript application is to extract all 
user-facing text strings from our code and store them in external 
resource files or databases.

Create an en.json file and add key value pairs for text strings used in the app. We can support additional languages by creating more translation files ex: fr.json, de.json 

#### Utilize third-party localization libraries

One of the easiest ways to implement i18n in a large JavaScript 
application is to use a third-party library like react-intl or i18next.

```jsx
import { useState } from 'react';
import { IntlProvider, FormattedMessage } from "react-intl";

import messages_en from "./locales/en.json";
import messages_es from "./locales/es.json";
import messages_fr from "./locales/fr.json";
import messages_de from "./locales/de.json";

const messages = {
 en: messages_en,
 es: messages_es,
 fr: messages_fr,
 de: messages_de,
};

export default function App() {
   const [locale, setLocale] = useState('en');

   return (
     <IntlProvider
       locale={locale}
       messages={messages[locale]}
      >
        <FormattedMessage id="greeting" />

        <div>
           <button onClick={() => setLocale("en")}>
             English
           </button>
           <button onClick={() => setLocale("es")}>
             Español
           </button>
           <button onClick={() => setLocale("fr")}>
             Français
           </button>
           <button onClick={() => setLocale("de")}>
             Deutsch
           </button>
         </div>
      </IntlProvider>
   );
}
```

#### Dynamic loading

In the code example above, we’re directly importing all language files in 
the root of our app, which can sometimes result in having all of these 
files included in the main bundle.

Dynamically importing locales

```jsx
import React, { useState, useEffect } from "react";
import { 
 IntlProvider, 
 FormattedMessage
} from "react-intl";

export function App() {
  const [locale, setLocale] = useState("en");
  const [messages, setMessages] = useState({});

  useEffect(() => {
    /*
     Dynamically import required translation file
     based on locale
    */

    const loadMessages = async () => {
      switch (locale) {
        case "en":
          await import(
            "./locales/en.json"
          ).then((m) => setMessages(m.default));
          break;
  
        case "es":
          await import(
            "./locales/es.json"
          ).then((m) => setMessages(m.default));
          break;
  
        // and so on...
      }
    };

    loadMessages();
  }, [locale])

  return (
     <IntlProvider
       locale={locale}
       messages={messages}
      >
        <FormattedMessage id="greeting" />

        <div>
           <button onClick={() => setLocale("en")}>
             English
           </button>
           <button onClick={() => setLocale("es")}>
             Español
           </button>
           <button onClick={() => setLocale("fr")}>
             Français
           </button>
           <button onClick={() => setLocale("de")}>
             Deutsch
           </button>
         </div>
      </IntlProvider>
   );
}
```

#### Handling plurals across languages

When building a web application for a global audience, it’s important to 
recognize that simple pluralization rules, like adding an ‘s’ to denote 
more than one in English, don’t apply universally. Different languages 
have distinct rules for plurals, and some can be more complex than 
others. For example, the Arabic language often has different plural rules 
for quantifying zero, one, two, few, many, etc.

In en.json file

```json
{
 "itemCountMessage": "{itemCount, plural, =0 
 {No items} one {1 item} other {You have 
 {itemCount} items}}"
}
```

The above syntax is ICU message syntax that builds on [ICU Message Formatting](https://unicode-org.github.io/icu/userguide/format_parse/messages/) - a standardized way to build and format messages in software, especially in the context of internationalization and localization.

```jsx
import { FormattedMessage } from 'react-intl';

function PluralComponent({ itemCount }) {
  return (
   <FormattedMessage
     id="itemCount"
     values={{ itemCount }}
   />
 );
}
```

When itemCount is 0 we'll see "No items", when item count is 1 we'll see "1 item" and if the value is > 1 we see "You have 2 items"

If we are maintianing Arabic 

```json
"itemCountMessage": "{itemCount, plural, =0 {لیس لدیك أي عناصر} one {لدیك عنصر واحد}
two {لدیك عنصرین} few {{itemCount} عناصر دیك}
many {لدیك العدید من العناصر} other
{{itemCount} عنصرًا}
```

In Arabic the grammer is different when saying items between 3-10(few) and 11-99(many)

#### Format dates, times, and numbers

Dates, times, and numbers often have different formats across locales. As 
a result, we should always make use of localization libraries or built-in 
language features available in our programming language to ensure 
accurate and localized formatting of dates, times, and numbers.

Formatting dates with the Intl JavaScript object

```js
const date = new Date();

const options = {
 year: "numeric",
 month: "long",
 day: "numeric",
};

const formattedDate = new Intl.DateTimeFormat(
  "en-US",
  options,
).format(date);

console.log(formattedDate);
// Output: June 30, 2023

const number = 1004580.89;

const formattedNumberDE = new Intl.NumberFormat(
  "de-DE",
).format(number);
const formattedNumberUS = new Intl.NumberFormat(
  "en-US",
).format(number);

// Output: 1.004.580,89
console.log(formattedNumberDE);

// Output: 1,004,580.89
console.log(formattedNumberUS);
```

react-intl library gives FormattedDate component

```jsx
import { FormattedDate } from "react-intl";

// 01/15/2026
<FormattedDate
  value={new Date()}
  locale="en-US"
/>
```

In addition to dates, react-intl also offers components like 
FormattedTime and FormattedNumber for localizing times and numbers, 
respectively. Here’s an example of using the FormattedNumber
component to format numbers differently between the United States and 
Germany

#### Consider Right-to-Left (RTL) Languages

