---
subjects:
- react
- nextjs
- javascript
- accesibility
title: Adding an HTML language attribute to a NextJS app
subtitle: ''
blurb: ''
date: 2021-01-08T00:00:00.000+00:00

---
Recently setting up a new NextJS app I wanted to include accessibility checking right from the start. It's much easier to stay on top of basic accessibility good practice with automated checking in place before you start rather than trying to make fix things up once you already have a bunch of code and UI in place.

Running [pa11y](https://pa11y.org/ "Pally docs") on the output of [create-next-app](https://nextjs.org/docs/api-reference/create-next-app "Create Next App docs") I was surprised that I got an error telling me I should have specified a `lang` attribute on the `html` element to tell clients what language the content was going to be in i.e. the rendered html tag looked like this `<html>` when it should ideally have looked like this `<html lang="en">` or indeed `<html lang="cy">` etc. Thinking about it, this makes sense, it would be parochial for NextJS devs to assume all people using the framework are making sites in english or any other language probably shouldn't. However, for a NextJS newbie like me it's not obvious how to go about setting it up properly.

The Answer

***

Tangents:

* Theres a [translate](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/translate "Mozdev translate docs") global attribute

  > an enumerated attribute that is used to specify whether an element's _translateable attribute_ values and its [`Text`](https://developer.mozilla.org/en-US/docs/Web/API/Text) node children should be translated when the page is localized, or whether to leave them unchanged.
* A big list of lang attribute values
  |Language|Code|
  |-|-|
  |Afrikaans|af|
  |Albanian|sq|
  |Arabic (Algeria)|ar-dz|
  |Arabic (Bahrain) |ar-bh|
  |Arabic (Egypt)|ar-eg|
  |Arabic (Iraq)|ar-iq|
  |Arabic (Jordan)|ar-jo|
  |Arabic (Kuwait)|ar-kw|
  |Arabic (Lebanon)|ar-lb|
  |Arabic (Libya)|ar-ly|
  |Arabic (Morocco)|ar-ma|
  |Arabic (Oman)|ar-om|
  |Arabic (Qatar)|ar-qa|
  |Arabic (Saudi Arabia)|ar-sa|
  |Arabic (Syria)|ar-sy|
  |Arabic (Tunisia)|ar-tn|
  |Arabic (U.A.E.)|ar-ae|
  |Arabic (Yemen)|ar-ye|
  |Basque|eu|
  |Belarusian|be|
  |Bulgarian|bg|
  |Catalan|ca|
  |Chinese (Hong Kong)|zh-hk|
  |Chinese (PRC)|zh-cn|
  |Chinese (Singapore)|zh-sg|
  |Chinese (Taiwan)|zh-tw|
  |Croatian|hr|
  |Czech|cs|
  |Danish|da|
  |Dutch (Belgium)|nl-be|
  |Dutch (Standard)|nl|
  |English|en|
  |English (Australia)|en-au|
  |English (Belize)|en-bz|
  |English (Canada)|en-ca|
  |English (Ireland)|en-ie|
  |English (Jamaica)|en-jm|
  |English (New Zealand)|en-nz|
  |English (South Africa)|en-za|
  |English (Trinidad)|en-tt|
  |English (United Kingdom)|en-gb|
  |English (United States)|en-us|
  |Estonian|et|
  |Faeroese|fo|
  |Farsi|fa|
  |Finnish|fi|
  |French (Belgium)|fr-be|
  |French (Canada)|fr-ca|
  |French (Luxembourg)|fr-lu|
  |French (Standard)|fr|
  |French (Switzerland)|fr-ch|
  |Gaelic (Scotland)|gd|
  |German (Austria)|de-at|
  |German (Liechtenstein)|de-li|
  |German (Luxembourg)|de-lu|
  |German (Standard)|de|
  |German (Switzerland)|de-ch|
  |Greek|el|
  |Hebrew|he|
  |Hindi|hi|
  |Hungarian|hu|
  |Icelandic|is|
  |Indonesian|id|
  |Irish|ga|
  |Italian (Standard)|it|
  |Italian (Switzerland)|it-ch|
  |Japanese|ja|
  |Korean|ko|
  |Korean (Johab)|ko|
  |Kurdish|ku|
  |Latvian|lv|
  |Lithuanian|lt|
  |Macedonian (FYROM)|mk|
  |Malayalam|ml|
  |Malaysian|ms|
  |Maltese|mt|
  |Norwegian|no|
  |Norwegian (Bokmål)|nb|
  |Norwegian (Nynorsk)|nn|
  |Polish|pl|
  |Portuguese (Brazil)|pt-br|
  |Portuguese (Portugal)|pt|
  |Punjabi|pa|
  |Rhaeto-Romanic|rm|
  |Romanian|ro|
  |Romanian (Republic of Moldova)|ro-md|
  |Russian|ru|
  |Russian (Republic of Moldova)|ru-md|
  |Serbian|sr|
  |Slovak|sk|
  |Slovenian|sl|
  |Sorbian|sb|
  |Spanish (Argentina)|es-ar|
  |Spanish (Bolivia)|es-bo|
  |Spanish (Chile)|es-cl|
  |Spanish (Colombia)|es-co|
  |Spanish (Costa Rica)|es-cr|
  |Spanish (Dominican Republic)|es-do|
  |Spanish (Ecuador)|es-ec|
  |Spanish (El Salvador)|es-sv|
  |Spanish (Guatemala)|es-gt|
  |Spanish (Honduras)|es-hn|
  |Spanish (Mexico)|es-mx|
  |Spanish (Nicaragua)|es-ni|
  |Spanish (Panama)|es-pa|
  |Spanish (Paraguay)|es-py|
  |Spanish (Peru)|es-pe|
  |Spanish (Puerto Rico)|es-pr|
  |Spanish (Spain)|es|
  |Spanish (Uruguay)|es-uy|
  |Spanish (Venezuela)|es-ve|
  |Swedish|sv|
  |Swedish (Finland)|sv-fi|
  |Thai|th|
  |Tsonga|ts|
  |Tswana|tn|
  |Turkish|tr|
  |Ukrainian|uk|
  |Urdu|ur|
  |Venda|ve|
  |Vietnamese|vi|
  |Welsh|cy|
  |Xhosa|xh|
  |Yiddish|ji|
  |Zulu|zu|