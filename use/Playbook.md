---
description: >-
  This page will help the user to get themselves onboarded on Anuvaad and
  perform an operation.
---

# Playbook

## Account Creation

{% hint style="info" %}
<mark style="color:red;">The automated onboarding process is disabled for now to restrict resource usage. For time being, please fill out the details</mark> [<mark style="color:blue;">here</mark>](https://users.anuvaad.org/user/request-signup) <mark style="color:red;">and we will reach back to you soon with login details (Remember to check spam as well). please send a mail in case you didn't receive any response within a day.</mark>&#x20;
{% endhint %}

Users shall onboard themselves on Anuvaad via the link below:\
\
Registration: [https://users.anuvaad.org/user/signup#](https://users.anuvaad.org/user/signup)

Once a user reaches the Sign Up page, they have to fill in the required details as shown below:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p><a href="https://users.anuvaad.org/user/signup">https://users.anuvaad.org/user/signup#</a></p></figcaption></figure>

Upon successful submission, an E-Mail will be sent to the registered ID with a verification link

{% hint style="info" %}
Please check the spam folder as well for authentication E-Mail
{% endhint %}

Clicking the verification link will redirect the user to the login page as shown below

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-25 23-22-38.png" alt="" width="563"><figcaption><p>login screen</p></figcaption></figure>

For security purposes, Anuvaad follows an OTP-based login mechanism. You will be asked for the E-Mail to which a one-time password will be sent.

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-25 23-23-11 (1).png" alt="" width="563"><figcaption><p>E-Mail screen</p></figcaption></figure>

Upon confirming the ID, you will be asked to opt for one of the authentication methods.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-25 23-28-18 (1).png" alt="" width="536"><figcaption><p>choose Authentication method</p></figcaption></figure>

{% hint style="info" %}
Note that the selected authentication method could be changed later on as well.
{% endhint %}

Everything discussed above is a one-time process and must be applicable only on initial login to the application. Going forward, you will be redirected to the OTP verification page upon successful login.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-25 23-24-04.png" alt="" width="563"><figcaption><p>OTP based login</p></figcaption></figure>

Upon providing the correct OTP, the user will reach the below landing page of Anuvaad and we are good to go!

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-25 23-24-43.png" alt=""><figcaption><p>Landing page</p></figcaption></figure>

## Translate Sentence

The translate sentence feature enables the user to input a text and instantaneously get its translation in another language. To use it, simply click on the **Translate Sentence** option on the landing page of Anuvaad.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-26 01-12-38.png" alt="" width="563"><figcaption><p>Quick translate feature</p></figcaption></figure>

The user will have to select the source language to which he provides the input and the target language to which the text must be translated.&#x20;

If an Indic language is selected as the source, by default _Transliteration_ feature will be enabled, assisting the user to type in that particular language from the normal keyboard with ease.&#x20;

Upon clicking submit, the translated sentence will be displayed along with the model used to perform the translation. Using this feature, a stakeholder can quickly check the accuracy of the translation performed by Anuvaad.

## Digitize Document

Digitize Document feature helps to convert scanned documents into editable digital format by preserving the structure. This process recognizes text in scanned (non hand-written for now) documents and converts it into searchable text.

To perform a document digitization, click on the **Digitize Document** option on the landing page of Anuvaad. You will be greeted with a screen as below

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-27 00-08-34.png" alt=""><figcaption><p>upload file</p></figcaption></figure>

User may select a document/image and choose the appropriate source language and then trigger the digitization process. A pop-up window appears which shows the progress of the ongoing process. This is an async job and will happen in the background. The time taken will be dependent on the nature of the uploaded file

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-27 00-28-55.png" alt="" width="563"><figcaption><p>progress screen</p></figcaption></figure>

The status of all tasks given by user to Anuvaad can be viewed on the [Digitization Dashboard](https://users.anuvaad.org/document-digitization) as below

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-27 00-23-41.png" alt=""><figcaption><p>Digitization dashboard</p></figcaption></figure>

If the status of a job is completed, you can view the result and make changes by clicking on the **view document icon**, which is second in the last column under the label **Action.**

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-27 00-21-31.png" alt=""><figcaption></figcaption></figure>

Users can make changes, if any, by double-clicking on the word. once done, the digitized document can be downloaded by clicking on the download button in the top right corner in the desired format.

{% hint style="info" %}
Anuvaad digitization works well on documents that have non-selectable yet printed content. It is mostly tested in scanned files
{% endhint %}

## Translate Document

The translate document feature enables the user to upload a document and get its translated version. The key highlight of the feature is that Anuvaad tries to maintain the original structure of the document in the best possible manner.

To perform a document translation, click on the **Translate Document** option on the landing page of Anuvaad. You will be greeted with a screen as below

{% hint style="info" %}
**Pro tip**: If the document to be translated does not contain unicode fonts, please perform document digitization and then translate the digitized document.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-26 01-23-22.png" alt=""><figcaption><p>upload screen</p></figcaption></figure>

Here, the user will have the provision to upload a document (pdf,docx,pptx formats are supported as of now)  and select a source and target language to perform translation. On successful upload of a supported file, the process begins and status will be shown

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-26 01-26-04.png" alt="" width="563"><figcaption><p>progress screen</p></figcaption></figure>

There are various stages happening behind the scenes of document translation and the status will be displayed on screen. The total time taken to complete the process depends on the number of pages and the structure of the input document. The translation is an `async` process and once initiated it will be performed in the background. Users can keep on adding more and more tasks to Anuvaad meanwhile.&#x20;

After a certain time, by going to the [Translation Dashboard](https://users.anuvaad.org/view-document), the user can access the translated document.

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-26 01-32-26.png" alt=""><figcaption></figcaption></figure>

Users can make necessary changes to the document using Anuvaad's easy editor which is developed by seeing document translators forefront, and later on download the translated document back to their system in the desired format.&#x20;

The blue icon in the bottom right corner is to use the merge feature. In case two or more text blocks need to be combined into a single unit, it could be used.&#x20;

## Glossary Support

Being an AI-assisted translation software, some occurrences can happen where a machine translation can give meaningful, yet non-contextual output to certain words/phrases. In order to work around this, Anuvaad offers user-level Glossary support.

A translator can store certain phrases and their predefined translation so that if there is an occurrence of a similar phrase in the document for translation, the system acts wisely based on the predefined criterions.&#x20;

The list of default Glossaries and added ones can be viewed on [my glossary](https://users.anuvaad.org/my-glossary) page.&#x20;



<figure><img src="../.gitbook/assets/Screenshot from 2023-09-28 20-46-14.png" alt=""><figcaption><p>List of available Glossaries</p></figcaption></figure>

To delete a glossary, simply click on the bin icon under the Action column.  To add an extra item, click on the [Create Glossary](https://users.anuvaad.org/user-glossary-upload) button in the top right corner. You will be redirected to the following page where new words/phrases and their corresponding translation can be added.  Once submitted, the new items will be visible on [my glossary](https://users.anuvaad.org/my-glossary) page.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-28 20-52-24 (1).png" alt="" width="563"><figcaption><p>Add a new Glossary</p></figcaption></figure>

## Analytics

Anuvaad also offers a built-in Analytics feature to keep track of usage metrics. These Analytics are instance-specific. Bar graph-based representations offer quick insights into how well Anuvaad is utilized. This also helps stakeholders to keep track of the number of Documents processed, Languages used, sentences translated, and organization-level information. Furthermore, these data could be exported in the desired format for future reference.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-09-28 20-25-38.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
All macro-level features to enhance translation speed are explained in detail in the Tutorial videos section.
{% endhint %}

