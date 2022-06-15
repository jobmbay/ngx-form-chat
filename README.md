# ngx-form-chat

> Transform your forms as a chatbot. ngx-form-chat allows you to customize everything with you chatbot.
> Now you no more need a long form which can be complex and bored. 

This module is actually avalaibled only in Angular.

See example with all configurations on [https://ngx-form-chat.web.app](https://ngx-form-chat.web.app)

## Install

Using NPM

```bash
npm install --save ngx-form-chat
```

Using Yarn

```bash
yarn add ngx-form-chat
```

## Usage

### app.module.ts

```jsx

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { NgxFormChatModule } from 'ngx-form-chat';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    NgxFormChatModule,
    ...
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

### app-component.ts

```jsx
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <ngx-form-chat [data]="chatbotData" [config]="config" (onEndChat)="handleResult($event)"></ngx-form-chat>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'ng-form-chat-demo';

  config: any = {
    configuration: {
        compagny: {
            logo: "assets/images/FDT-logo.png",
            name: "Default Company",
            logoWidth: "230px"
        },
        background: "#eee",
        fontSize: "16px",
        chatpanel: {
            autoopen: false,
            restartChatWhenOpen: false,
            useSession: false,
            buttonColor: "#FFA800",
            position: {
                width: "500px",
                bottom: "65px",
                height: "70vh"
            }
        },
        userInput: {
            placeholder: "Your answer here",
            buttoncolor: "#0c2d62",
            color: "#ffffff"
        }
    },
    chatbot: {
        name: "Fatima",
        icon: "assets/images/fatima.png",
        background: "#0c2d62",
        color: "#ffffff"
    },
    user: {
        name: "You",
        icon: "assets/images/user.png",
        background: "#FFA800",
        color: "#ffffff"
    }
  }

  chatbotData: any = {
    questions: [
        {
            code: "firstname",
            type: "text",
            next: "lastname",
            messageTexts: [
                "Hello, <br>my name is Fatima and I <strong>welcome you to ngx-form-chat .</strong>",
                "I am the artificial intelligence (AI) of ngx-form-chat in charge of transmitting your request to the loan of ngx-form-chat in few seconds ⏱.",
                "To begin with, what is your ? <strong>first name</strong> please ?"
            ],
            validator: {
              type: "text",
              messages: [
                "Please enter your first name with minimum 3 characters",
                "Numbers are not allowed"
              ],
              min: 3,
              max: Infinity,
              regExp: /^[a-zA-Z ]{3,}$/
            }
        },
        {
            code: "lastname",
            type: "text",
            next: "email",
            messageTexts: [
                "Well done <strong>FIRSTNAME !</strong>",
                "Now, can you tell me your  <strong> last name</strong> please ?",
                "Please mention it below."
            ],
            variables: [
                {
                    cle: "FIRSTNAME",
                    referenceCode: "firstname"
                }
            ],
            validator: {
              type: "text",
              messages: [
                "Please enter your last name with minimum 2 characters",
                "Numbers are not allowed"
              ],
              min: 2,
              max: Infinity,
              regExp: /^[a-zA-Z ]{2,}$/
            }
        },
        {
            code: "email",
            type: "text",
            next: "organisation",
            messageTexts: [
                "Perfect <strong>FIRSTNAME LASTNAME !</strong>",
                "What is your Email please ?"
            ],
            variables: [
              {
                  cle: "FIRSTNAME",
                  referenceCode: "firstname"
              },
              {
                cle: "LASTNAME",
                referenceCode: "lastname"
              }
            ],
            validator: {
              type: "email",
              messages: [
                "Email address is not valid",
                "Please enter your email address as ex: example@domaine.abc"
              ]
            }
        },
        {
            code: "organisation",
            type: "option",
            messageTexts: [
                "Thank you very much, it's almost over FIRSTNAME!",
                "In order to better personalize our relationship, can you tell me, by clicking, the <strong>type of your company or organization</strong>?"
            ],
            variables: [
              {
                cle: "FIRSTNAME",
                referenceCode: "firstname"
              }
            ],
            options: [
                {
                    value: "A company",
                    next: "organizationName"
                },
                {
                    value: "A big account",
                    next: "organizationName"
                },
                {
                    value: "A digital agency",
                    next: "organizationName"
                },
                {
                    value: "A consultant",
                    next: "phoneNumber"
                }
            ]
        },
        {
          code: "organizationName",
          type: "text",
          next: "organizationSize",
          messageTexts: [
              "Good! I see that you are <strong>ORGANISATION</strong>",
              "Can you please tell me the <strong>name of your organization</strong>?"
          ],
          variables: [
            {
              cle: "ORGANISATION",
              referenceCode: "organisation"
            }
          ],
          validator: {
            type: "text",
            messages: [
              "Please enter your organization name with minimum 2 characters"
            ],
            min: 2,
            max: Infinity
          }
        },
        {
          code: "organizationSize",
          type: "text",
          next: "phoneNumber",
          messageTexts: [
              "It is an honor for us to work with <strong>ORGANISATION_NAME</strong>",
              "How many members are there at <strong>ORGANISATION_NAME</strong>?"
          ],
          variables: [
            {
              cle: "ORGANISATION_NAME",
              referenceCode: "organizationName"
            }
          ],
          validator: {
            type: "number",
            messages: [
              "Please enter your organization size with only numbers"
            ]
          }
        },
        {
          code: "phoneNumber",
          type: "text",
          next: "salutation",
          messageTexts: [
              "Now this is the last question <strong>FIRSTNAME</strong>",
              "Can you please tell me your <strong>phone number</strong>?"
          ],
          variables: [
            {
              cle: "FIRSTNAME",
              referenceCode: "firstname"
            }
          ],
          validator: {
            type: "phone",
            messages: [
              "Please enter your phone number in correct format",
              "Indicator is required, ex: +221 7XXXXXXXX"
            ]
          }
        },
        {
          code: "salutation",
          type: "end",
          messageTexts: [
              "Thank you for your time <strong>FIRSTNAME</strong>",
              "I will be in touch with you shortly."
          ],
          variables: [
            {
              cle: "FIRSTNAME",
              referenceCode: "firstname"
            }
          ]
        }
    ]
  }

  handleResult(result: any) {
    console.log(result);
  }
  
}

```



## Config proprieties

Propriety | Type | Default | Description
--- | --- | --- | ---
logo | string | -- | This is the uri of the logo used in the chat panel
name | string | -- | Name of company, project or website
logoWidth | string | "230px" | Width of the use logo
background | string | "#eee" | Background of the chatpanel
fontSize | string | "16px" | FontSize of the text in the chat messages
autoopen | boolean | false | The chat panel is automatically open when true
restartChatWhenOpen | boolean | false | Restart the chat at anytime that the chat panel is opened when true
useSession | boolean | false | Use localSession on the browser to store chat process when true
buttonColor | string | "#FFA800" | Color of the buttom that is used to open and close the chat panel
width | string | "500px" | Width position of the chat panel
bottom | string | "65px" | Bottom position of the chat panel
height | string | "70vh" | Height position of the chat panel
userInput.placeholder | string | "Your answer here" | The placeholder used in the user input
userInput.buttoncolor | string | "#0c2d62" | Background of the button which send user's answer
userInput.color | string | "#ffffff" | Color of the button which send user's answer
chatbot.name | string | "Fatima" | Name of chatbot
chatbot.icon | string | "assets/images/fatima.png" | Icon of chatbot
chatbot.background | string | "#0c2d62" | Background of chatbot messages
chatbot.color | string | "#ffffff" | Color of chatbot messages
user.name | string | "You" | Name of user
user.icon | string | "assets/images/user.png" | Icon of user
user.background | string | "#0c2d62" | Background of user messages
user.color | string | "#ffffff" | Color of user messages

<hr size="1">



## data proprieties

Propriety | Type | Description
--- | --- | --- 
code | string | Code of question that will be used as propriety in the result's object
type | string | Type of question 'text', 'option' or 'end'
next | string | The next code of question to be sent by the chatbot
messageTexts | array of string | All lines in a message
variables | array of object | Variables to be replaced in the lines of the message
variables.cle | string | Key of the Variable to be searched in the lines of the message
variables.referenceCode | string | code of the question where the previous answer is to be replaced in the lines of the message
options | array of object | All possible answer of the question
options.value | string | Value of the option
options.next | string | The next code of question to be sent by the chatbot when the option is selected
validator | object | All Validations all user answer
validator.type | object | Type of user answer
validator.min | number | Minimum of characters if string and minimum of value input if number 
validator.max | number | Maximum of characters if string and maximum of value input if number 
validator.messages | array of string | List of messages to be sent when error format on user answer
validator.regExp | string | Regex for additionnal regex control

<hr size="1">


## Author

<img src="https://media-exp1.licdn.com/dms/image/C4D03AQGWvPsvVHor2Q/profile-displayphoto-shrink_800_800/0/1631823890576?e=1660780800&v=beta&t=XoifiXevu54i36f7ciIraok7UutDu8oijO9iEI0biJQ" data-canonical-src="https://www.linkedin.com/in/mbaye-diop-b52330120/" width="25" height="25" style="border-radius: 50%" /> Mbaye DIOP

This package is actually maintained by Mbaye DIOP.

LinkedIn: https://www.linkedin.com/in/mbaye-diop-b52330120/
