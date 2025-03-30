# What are Cookies?

Cookies are data that browsers store in small text files on your computer.

Cookies were invented to solve the problem of "how to remember information about the user?". 

When you sign in to a web application like CyberTalents, you won’t need to type your email and password again next time because it stores your credentials in the cookies. 

Cookies are saved in name-value pairs like:

|   |
|---|
|username = user|

When a browser requests a web page from a server, cookies belonging to the page are added to the request. This way the server gets the necessary data to "remember" information about users.

## Cookies Vs Sessions

### Cookies

- Stored on client-side.
- Lives longer even if the browser is closed.
- Less secure. 
- Can only store strings. 

### Sessions 

- Stored on the server side.
- Expires when the browser is closed.
- More secure. 
- Can store objects.

# Cookies Manipulation

## Create a Cookie with JavaScript

JavaScript can create, read, and delete cookies with the document.cookie property.

With JavaScript, a cookie can be created like this:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfx2oc79CGWEPLf3xfTppVTyj9wPZ9caLWd2YH7BAp6ivBCMGjBWRwACbuNZ6JuqeVSW9Wh3S7jd414rdVpodwrqsvEpXkq3qIcVn9R6jrfqcawQvPfbmwdBdgzujJcxZo2GVgwbOXuDJu6neyK8EyqAkU?key=A6MVxPjtWIV2Olzi3eSQ4Q)

You can also add an expiry date (in UTC time). By default, the cookie is deleted when the browser is closed:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe6kXWprSQHBIJyTqhfNoF_fkCC-p28p_tN_64Yy35QVUUmH9-JeB2WGN1ELSXsqgJOc3EDQZy1VreaJVKyThCcI58c80-ef0rdaGBNreITXcSJR587Dgk6kv11aWQ5WPnciDUD6776XpXp8o1KX1Qz3B4?key=A6MVxPjtWIV2Olzi3eSQ4Q)

With a path parameter, you can tell the browser what path the cookie belongs to. By default, the cookie belongs to the current page.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdcKOqsh0lp4f08JLu91aW2Qz24-AP09qhcmtZ1pGzE4b2cy5WIvbj9Abi3xkvsRlaseK95gQdyGNI8iwB3FDU3jPsp-3elST3vdGUn3QdQoD3i6vghesDNQoxwnuG_Aq7qzszKXkarReGtgpnrYhPh4SNN?key=A6MVxPjtWIV2Olzi3eSQ4Q)

## Read a Cookie with JavaScript

With JavaScript, cookies can be read like this:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfU2hiQhOHN60WPHmSPooATRTFE4D52hOdR-1eedjMZ5cYO2-ecHrIqYBmbsKVx3cw_gbCaNqcBaeqwucRzde8QQ63VwPe_wNdSNgCG7f61x6C6GJD__ym9ppxg3y4izBVX4C59uJDu1Ov4BLWk-55V-5Y?key=A6MVxPjtWIV2Olzi3eSQ4Q)

## Change a Cookie with JavaScript

With JavaScript, you can change a cookie the same way as you create it:

## ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfO1pHeWlfz7xXQhDo5114QdroT5oX2_frbe9AfDuyfxl6XUnk5AC6ZHZ4v_uo6XoSK8DWX_Yi8OUGY2xzonVT7Stu4nUNr1Ya2XjdyXVms5hgClWMPWphgvkamg2_7ZksvDnTcoDUcEiYR4ltyawXlEx65?key=A6MVxPjtWIV2Olzi3eSQ4Q)

## Delete a Cookie with JavaScript

Deleting a cookie is very simple. You don't have to specify a cookie value when you delete a cookie. Just set the expires parameter to a passed date:

|   |
|---|
|document.cookie = "username=test; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";|

## A Function to Set a Cookie

First, we create a function that stores the name of the visitor in a cookie variable:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf_N6pQmNZcDUMecABhFiBCHVHSXmCuoiNL823WdIrUdMwXOZtLot3Zk1OgITbFcT8zYV0Fuzkt4v05_tFhmRi9CHsdrTQXaf9bDVj8TVD0BU9-TJvsTW4XYwvgfmLTa_GQLhWg1GTeXZjGFqKjmjym_1g?key=A6MVxPjtWIV2Olzi3eSQ4Q)

### Example Explained:

The parameters of the function above are the name of the cookie (cname), the value of the cookie (cvalue), and the number of days until the cookie should expire (exdays).

The function sets a cookie by adding together the cookie name, the cookie value, and the expiration string.

## View and Edit Page Cookies

On Firefox from the storage tab, you can view the page cookies.  
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfTzEs79RZw0Qnorb3DG5JhLvVJuVRKAfjhvo-MqGuB8Sl1fWncJzpyLYNUzsxqODLfmjjwkqKzJrVbWrQW24urn9syTWLQl53JKmJSZChuJcMnbvaiISM4Qu_3zZymeCJTd_xMS7oi1rFv9fTWgz7yVn4?key=A6MVxPjtWIV2Olzi3eSQ4Q)

On Chrome, you will find the page cookies in the application tab.

# ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcLMNiNUMQI0L5pd8VsWOMpM3m0frTVOw8alqaD8nwbT6ybdiRM3Obkd7vPTYwDdcTwsLEKxdKPujLKIcytmvZUEcFhoW3I7P3WSxERYVMpZbBJrxnqDPatTvjvL2orp6vjE7td4ZqIs3O-nOIB1GgpDAcq?key=A6MVxPjtWIV2Olzi3eSQ4Q)