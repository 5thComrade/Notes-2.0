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
}
```

#### Dynamic loading

