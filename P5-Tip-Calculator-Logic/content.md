---
title: "Tip Calculator Logic"
slug: tip-calculator-logic
---

At this point, we've finished building our tip calculator's UI. However, if we launch our app and try to use it, nothing happens. Hmm... not particularly useful.

To prevent angry users and one-star reviews, we'll make our tip calculator work by writing code that:

1. takes the user's input
1. calculates the correct outputs
1. updates the UI with the output data

Let's start by figuring out how to access our user's original bill amount as input!

# Reading Bill Amount Input

To calculate our output, we'll first need the user to input their original bill amount.

![Bill Amount Input](assets/bill_amount_input.png)

When the user taps on the _Bill Amount Text Field_, the decimal pad keyboard will be displayed for user input. However, after the user is done typing their bill amount, there's no way for them to dismiss the keyboard.

We'll fix this first, by implementing a _Calculate Button_ that will appear right above the keyboard.

> [info]
To keep things simple, there's a bit of _magic_ that happens behind these scene for our soon-to-be calculate button. We've abstracted the majority into a custom `UITextField` subclasses which will replace our current `UITextField`. Even if you don't fully understand how our calculate button is being displayed, bear with us for now!

## Adding a Calculate Button

> [action]
Open `BillAmountTextField.swift` from your _Project Navigator_. You should see the following:
>
![Bill Amount Text Field Swift Code](assets/bill_amount_text_field_swift.png)

The biggest thing to note, is that `BillAmountTextField.swift` is a custom subclass of `UITextField` that has an _input accessory view_, or a view that sits on top of the keyboard.

Don't worry too much about the code in this file for now. In short, it creates our _Calculate Button_, positions it within the's text field's _input accessory view_. We'll see that in a second.

To use our subclass, we'll need to replace our current `UITextField` with our `BillAmountTextField` subclass.

> [action]
Open `Main.storyboard` and set `BillAmountTextField` as our _Bill Amount Text Field's_ custom class.
>
1. Select the _Bill Amount Text Field_ using the _Document Outline_. ![Select Text Field](assets/select_text_field.png)
1. With the text field still selected, open the _Identity Inspector_ in the _Utilities area_. ![Open Identity Inspector](assets/open_identity_inspector.png)








 done or calculate button for them to dismiss the keyboard.

We'll fix that now.






In order to calculate our tip amount and total amount, we'll first need the user's input of how much their original bill cost.

look there's no done button

add calculate button (don't worry it's magic)

print output to console when pressing calculate button

dismiss first responder?? here or next section

<!-- TODO: text field needs input accessory view -->







# Calculating Tip

First let's implement our tip logic. We'll add this code into an extension. An extension is useful for organize the code in your source files. You can use extensions to group alike code.

> [action]
Let's create a `calculate(_:)` function that will contain our logic for calculating our tip when the user inputs a bill amount.
>
```
private func calculate() {
    guard let billAmountText = billAmountTextField.text,
        let billAmount = Double(billAmountText) else {
            return
    }
>
    // get tip percent
>
    // calculate tip amount and total bill amount
>
    // update UI
}
```
>
First, we'll need to make sure that there is a valid amount in bill amount text field.
>
```
private func calculate() {
    guard let billAmountText = billAmountTextField.text,
        let billAmount = Double(billAmountText) else {
            return
    }
>
    let tipPercentage: Double
    switch tipSegmentedControl.selectedSegmentIndex {
    case 0:
        tipPercentage = 0.15
    case 1:
        tipPercentage = 0.18
    case 2:
        tipPercentage = 0.20
    default:
        preconditionFailure("Unexpected index.")
    }
>
    // calculate tip amount and total bill amount
>
    // update UI
}
```
>
Next, we look at the correct tip percent based on the selected index of the segmented control.
>
```
private func calculate() {
    guard let billAmountText = billAmountTextField.text,
        let billAmount = Double(billAmountText) else {
            return
    }
>
    let tipPercentage: Double
    switch tipSegmentedControl.selectedSegmentIndex {
    case 0:
        tipPercentage = 0.15
    case 1:
        tipPercentage = 0.18
    case 2:
        tipPercentage = 0.20
    default:
        preconditionFailure("Unexpected index.")
    }
>
    let roundedBillAmount = roundAmount(billAmount)
    let tipAmount = roundedBillAmount * tipPercentage
    let roundedTipAmount = roundAmount(tipAmount)
    let totalAmount = roundedBillAmount + roundedTipAmount>
>
    // update UI
}
```
Next, we add the logic for each of the values we need. Notice we create a helper function for rounding our bill amount. Add the following helper method in the same extension:
>
```
private func roundAmount(_ amount: Double) -> Double {
    return round(100 * amount) / 100
}
```
Finally, we move onto the last step of updating the UI of our tip calculator:
>
```
private func calculate() {
    guard let billAmountText = billAmountTextField.text,
        let billAmount = Double(billAmountText) else {
            return
    }
>
    let tipPercentage: Double
    switch tipSegmentedControl.selectedSegmentIndex {
    case 0:
        tipPercentage = 0.15
    case 1:
        tipPercentage = 0.18
    case 2:
        tipPercentage = 0.20
    default:
        preconditionFailure("Unexpected index.")
    }
>
    let roundedBillAmount = roundAmount(billAmount)
    let tipAmount = roundedBillAmount * tipPercentage
    let roundedTipAmount = roundAmount(tipAmount)
    let totalAmount = roundedBillAmount + roundedTipAmount>
>
    // update UI
    billAmountTextField.text = String(format: "%.2f", roundedBillAmount)
    tipAmountLabel.text = String(format: "%.2f", roundedTipAmount)
    totalCostLabel.text = String(format: "%.2f", totalAmount)
}
```

We've now successfully implemented our logic for calculating our tip.


add logic in the right places
keyboard input accessory
clear logic
