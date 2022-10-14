---
coverY: 0
---

# Patch Function Error-Handling

### Symptoms

Good connectivity and the correct user permissions cannot be assumed when attempting to patch item(s) to a datasource.  Subsequent code dependant on the output of the patch function will fail. This results in a poor user experience such as unfriendly error messages being displayed to users.

### Guidance

Use [IsError](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-iferror) with the [Patch function](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-patch) or [Collect Function](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-clear-collect-clearcollect). This is not necessary for local collections stored in memory.

#### Incorrect examples

`// Create a new invoice and navigate to success screen`\
`Patch(`\
&#x20;   `'Invoices',`\
&#x20;   `Defaults('Invoices'),`\
&#x20;   `{`\
&#x20;       `CustomerNumber: "C0001023",` \
&#x20;       `InvoiceDate: Date(2022, 6, 13)` \
&#x20;       `PaymentTerms: "Cash On Delivery",` \
&#x20;       `TotalAmount: 13423.75`\
&#x20;   `}`\
`);`\
`Navigate(`\
&#x20;   `'Success Screen'`\
`)`

#### Correct examples

`// Create a new invoice`\
`If(`\
&#x20;   `IsError(`\
&#x20;       `Patch(`\
&#x20;           `'Invoices',`\
&#x20;           `Defaults('Invoices'),`\
&#x20;           `{`\
&#x20;               `CustomerNumber: "C0001023",` \
&#x20;               `InvoiceDate: Date(2022, 6, 13)` \
&#x20;               `PaymentTerms: "Cash On Delivery",` \
&#x20;               `TotalAmount: 13423.75`\
&#x20;           `}`\
&#x20;       `)` \
&#x20;    `),` \
&#x20;    `// On failure`  \
&#x20;    `Notify(`\
&#x20;        `"Error: the invoice could not be created",` \
&#x20;        `NotificationType.Error` \
&#x20;    `),` \
&#x20;    `// On success`  \
&#x20;    `Navigate(`\
&#x20;        `'Success Screen'`\
&#x20;    `)`\
`)`

### Version

This rule was introduced on 14/10/2022 and last updated 14/10/2022
