---
title: "Tip Calculator Logic"
slug: tip-calculator-logic
---

In the previous section, we implemented the basic layout for our tip calculator. In this section, we'll continue building our app by implementing the logic for calculating the tip given a bill amount.


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



