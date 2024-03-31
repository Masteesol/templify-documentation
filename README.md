# Documentation for the Templify App

## Navigation

**[Components](./Components/README.md)** |
**[Database](./Database/README.md)** |
**[API](./Api/README.md)**

## About Templify

### Background and use case

The template manager app is a lightweight template manager aimed at primarily customer service work. Marius used to work at IKEA at customer support. IKEA has systems for automatically sending SMS's to customers when everything goes as planned, but when handling deviations like delayed orders and case management like  handling claims, you can't rely on automatic systems as the cases vary greatly for each customer.

#### Inspiration for the design

At IKEA, Marius used sticky notes to hold text templates, with `XXX` as placeholders for customer names, order numbers, case numbers, etc. Using sticky notes became hard to manage and the fact that you had to double click to mark the `XXX` symbols and overwrite them meant it was prone to errors and inconsistencies.

```txt
Dear XXX,

We've tried to contact you regarding your case, XXX.

Best regards,
IKEA customer support
```

IKEA had a templating tool, which no one used, because it was hard to manage and didn't have the inline placeholders, like the Templify app has.

Taking inspiration from working with sticky notes, Marius made a prototype that had these cards with text, where you could add the text, and each `#` symbol would render an input field at the top of the card. When exiting editing mode for that card, you could use these input fields to add inline text, without having to double click `XXX` and overwrite the text, like before. Using this tool for a few months, Marius increased the consistency and speed of which he interacted with customers and logged the interactions.

The prototype was also used for customer chat, which is a high-paced task where the ideal number of concurrent chats are up to five, depending on the KPI's of that company. Working with multiple concurrent chat sessions, you have to be fast and precise, and your KPI's often are messured in time and customer satisfaction, which can be a stressfull factor. The templating tool greatly helped the speed and consistency of which Marius delivered repeated sentences.

The UI / UX of the prototype wasn't really designed for sentences without inline variable text, but still did the job. In the new version of the app, Templify, the prefered card for basic sentences without variable text is the template collection, which is mentioned further down in this document.

### Text examples

This is a typical message we had to send if an order had been delayed. The `#` symbols will render input fields for that card.

```txt
Dear #,

Your order # has unfortunately been delayed.

The new delivery time has been set to # between # and #.

Best regards,
IKEA customer support
```

Other cases could be logging, which benefits from a standard format. Let's look at claims logging:

```txt
Customer issue: #
Provided Solution: #
Third party service provider: #
Case status: #
Follow up date: #
```

Using this template keeps the logging to the point and brief.

## Template card types

The two main components you should know of are *single templates* and *template collections*. These two elements are similar, but provide different functionality and serve different purposes.

### Single Templates

With single templates you can add inline placeholders and are ideal for situations like email and sms templates, where information like names and case number/order number varies, but the rest of the text remains the same.

### Template Collections

Template collections are meant for smaller amount of text and is ideal for fast paced work situations like customer service chat, where you have basic sentences which are repeated for each customer.
