# Funsol Billing Helper

[![](https://jitpack.io/v/Funsol-Projects/Funsol-Billing-Helper.svg)](https://jitpack.io/#Funsol-Projects/Funsol-Billing-Helper)

Funsol Billing Helper is a simple, straight-forward implementation of the Android v5.1 In-app billing API

> Support both IN-App and Subscriptions.

### **Billing v5 subscription model:**

![Subcription](https://user-images.githubusercontent.com/106656179/227849820-8b9e8566-fa6e-40d4-862e-77aaeaa65e6c.png)

## Getting Started

#### Dependencies

No extra dependencies required

## Step 1

Add maven repository in project level build.gradle or in latest project setting.gradle file

```kotlin 

    repositories {
        google()
        mavenCentral()
        maven { url "https://jitpack.io" }
    }
 
```  

## Step 2

Add Funsol Billing Helper dependencies in App level build.gradle.

```kotlin

    dependencies {
        implementation 'com.github.Funsol-Projects:Funsol-Billing-Helper:v1.0.2' 
    }
 
```  

## Step 3 (Setup)

Finally initialise Billing class and setup Subscription Ids

```kotlin 

    FunSolBillingHelper(this)
    .setSubKeys(mutableListOf("Subs Key", "Subs Key 2"))
 
```
if both subscription and In-App

```kotlin 

    FunSolBillingHelper(this)
    .setSubKeys(mutableListOf("Subs Key", "Subs Key 2"))
    .setInAppKeys(mutableListOf("In-App Key")) 
  
```

Call this in first stable activity

### Enable Logs


```kotlin

    FunSolBillingHelper(this)
    .setSubKeys(mutableListOf("Subs Key", "Subs Key 2"))
    .setInAppKeys(mutableListOf("In-App Key"))
    .enableLogging()


```
### Buy In-App Product

Subscribe to a Subscription
```kotlin
    FunSolBillingHelper(this).buyInApp("In-App Key",false)
```
```fasle```  value used for **isPersonalizedOffer** attribute:

If your app can be distributed to users in the European Union, use the **isPersonalizedOffer** value ```true``` to disclose to users that an item's price was personalized using automated decision-making.

**Note: it auto acknowledge the In-App and give callback when product acknowledged successfully.**
### Subscribe to a Subscription

Subscribe to a Subscription
```kotlin
    FunSolBillingHelper(this).subscribe(this, "Base Plan ID")
```
Subscribe to a offer
```kotlin
    FunSolBillingHelper(this).subscribe(this, "Base Plan ID","Offer ID")
```

**Note: it auto acknowledge the subscription and give callback when product acknowledged successfully.**

### Upgrade or Downgrade Subscription

 ```kotlin
    FunSolBillingHelper(this).upgradeOrDowngradeSubscription(this, "New Base Plan ID", "New Offer Id (If offer )", "Old Base Plan ID", ProrationMode)

```
```ProrationMode``` is a setting in subscription billing systems that determines how proration is calculated when changes are made to a subscription plan. There are different proration modes, including:

```

  1. DEFERRED

    Replacement takes effect when the old plan expires, and the new price will be charged at the same time.

  2. IMMEDIATE_AND_CHARGE_FULL_PRICE

    Replacement takes effect immediately, and the user is charged full price of new plan and is given a full billing cycle of subscription, plus remaining prorated time from the old plan.

  3. IMMEDIATE_AND_CHARGE_PRORATED_PRICE

    Replacement takes effect immediately, and the billing cycle remains the same.

  4. IMMEDIATE_WITHOUT_PRORATION

    Replacement takes effect immediately, and the new price will be charged on next recurrence time.

  5. IMMEDIATE_WITH_TIME_PRORATION

    Replacement takes effect immediately, and the remaining time will be prorated and credited to the user.

  6. UNKNOWN_SUBSCRIPTION_UPGRADE_DOWNGRADE_POLICY
 

```
Example :

```kotlin
  FunSolBillingHelper(this).upgradeOrDowngradeSubscription(this, "New Base Plan ID", "New Offer Id (If offer )", "Old Base Plan ID", BillingFlowParams.ProrationMode.IMMEDIATE_AND_CHARGE_FULL_PRICE)
```

### Billing Listeners

Interface implementation to handle purchase results and errors.
 ```kotlin

      FunSolBillingHelper(this).setBillingEventListener(object : BillingEventListener {
            override fun onProductsPurchased(purchases: List<Purchase?>) {
            }

            override fun onPurchaseAcknowledged(purchase: Purchase) {
            }

            override fun onBillingError(error: ErrorType) {
                when (error) {
                    ErrorType.CLIENT_NOT_READY -> {

                    }
                    ErrorType.CLIENT_DISCONNECTED -> {

                    }
                    ErrorType.PRODUCT_NOT_EXIST -> {

                    }
                    ErrorType.BILLING_ERROR -> {

                    }
                    ErrorType.USER_CANCELED -> {

                    }
                    ErrorType.SERVICE_UNAVAILABLE -> {

                    }
                    ErrorType.BILLING_UNAVAILABLE -> {

                    }
                    ErrorType.ITEM_UNAVAILABLE -> {

                    }
                    ErrorType.DEVELOPER_ERROR -> {

                    }
                    ErrorType.ERROR -> {

                    }
                    ErrorType.ITEM_ALREADY_OWNED -> {

                    }
                    ErrorType.ITEM_NOT_OWNED -> {

                    }

                    ErrorType.SERVICE_DISCONNECTED -> {

                    }

                    ErrorType.ACKNOWLEDGE_ERROR -> {

                    }

                    ErrorType.ACKNOWLEDGE_WARNING -> {

                    }
                    
                    ErrorType.OLD_PURCHASE_TOKEN_NOT_FOUND -> {

                    }
                    else -> {

                    }
                }
            }
        })
 
```

## Step 4 (Product's Detail)

### Get Product price

Get all products prices list include both In-App and Subs

```kotlin
    FunSolBillingHelper(this).getAllProductPrices()
```
Get In-App Product price


```kotlin
    FunSolBillingHelper(this).getProductPriceByKey("In-App Key").price
```

Get specific subscription price (without offer)


```kotlin
    FunSolBillingHelper(this).getProductPriceByKey("Base Plan ID","").price
```

Get specific subscription price (with offer)


```kotlin
    FunSolBillingHelper(this).getProductPriceByKey("Base Plan ID","Offer ID").price
```

This method return ```ProductPriceInfo``` object that contain complete detail   about subscription. To get only price just call ```.Price```.

### Get Single Product Detail

For In-App Product

```kotlin
    FunSolBillingHelper(this).getProductDetail("In-App Key","",BillingClient.ProductType.INAPP)
```
For Subs Product

```kotlin
    FunSolBillingHelper(this).getProductDetail("Base Plan ID","Offer ID",BillingClient.ProductType.SUBS)
```

Above methods return ```ProductPriceInfo``` object that contain complete detail about Product.

## Step 5 (Check if any Product buy)

### Check In-App

For check if user buy any In-App Product

 ```kotlin
  FunSolBillingHelper(this).isInAppPremiumUser()

 ``` 

For check specific In-App Product

``` kotlin
  FunSolBillingHelper(this).isInAppPremiumUserByInAppKey("In-App Key")

 ```

### Check Subscription

For check if any subscription is subscribe

 ```kotlin
  FunSolBillingHelper(this).isSubsPremiumUser()

 ``` 

For check if any specific subscription is subscribe (by Base Plan ID)

``` kotlin
  FunSolBillingHelper(this).isSubsPremiumUserByBasePlanKey("Base Plan ID")

 ``` 
For check if any specific subscription is subscribe (by Subscription ID)

``` kotlin
  FunSolBillingHelper(this).isSubsPremiumUserBySubIDKey("Subscription ID")

 ``` 

## Step 6 (Cancel any subscription)

### Cancel  Subscription

```kotlin
FunSolBillingHelper(this).unsubscribe(this,"Subscription ID")
```

## Step 7 (Other Utils for Billing)

### Handle pending purchases

to check and handle pending purchase call this below method on activity ```OnResume()``` method.

```kotlin
FunSolBillingHelper(this).fetchActivePurchases()
```

**Note: Call this method in background thread. Use Coroutine to run this method in background.**

### Check subscription support

```kotlin
FunSolBillingHelper(this).areSubscriptionsSupported()

```
### Check Billing Client is Ready

```kotlin
FunSolBillingHelper(this).isClientReady()

```

### Release billing client object

Call this method when app close or when billing not needed any more.
```kotlin
FunSolBillingHelper(this).release()

```
This Method used for Releasing the client object and save from memory leaks


## License

#### MIT License
#### Copyright (c) 2023 [Hannan Shahid](https://github.com/hannanshahid)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and
associated documentation files (the "Software"), to deal in the Software without restriction,
including without limitation the rights to use, copy, modify, merge, publish, distribute,
sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial
portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT
NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES
OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

