# CI4FormBuilder

Library to build and manage forms in a CodeIgniter 4 projects (Object-Oriented way).

## Content

- [Instalation & Loading](#instalation-&-loading)
- [Components](#components)
- [How to use](#how-to-use)
    - [Basic Usage](#basic-usage)
    - [Using a Template](#using-a-template)

## Instalation & Loading

CI4FormBuilder is available on [Packagist](https://packagist.org/packages/rafaelwendel/ci4formbuilder), and instalation via [Composer](https://getcomposer.org) is the recommended way to install it. Add the follow line to your `composer.json` file:

```json
"rafaelwendel/ci4formbuilder" : "^0.0.1"
```

or run

```sh
composer require rafaelwendel/ci4formbuilder
```

## Components

The following form components are available to use.
- `Button`
- `Checkbox`
- `Dropdown`
- `Hidden`
- `Input`
- `Label`
- `Multiselect`
- `Password`
- `Radio`
- `Reset`
- `Submit`
- `Textarea`
- `Upload`

## How to use

### Basic Usage

The cited components can be instantiated and added to a Form.

```php
<?php

namespace App\Controllers;

use CI4FormBuilder\Form;
use CI4FormBuilder\Input;
use CI4FormBuilder\Label;
use CI4FormBuilder\Submit;

class Home extends BaseController
{
    public function index()
    {
        $form = new Form(); //the same constructor params of CI4 "form_open" function can be used

        //create an input to "firstname"
        $firstNameField = new Input('firstname'); //the same constructor params of CI4 "form_input" function
        //set a label to component
        $firstNameField->setLabel(new Label('First Name: ', 'firstName'));

        //create an input to "lastname"
        $lastNameField = new Input('lastname');
        $lastNameField->setLabel(new Label('Last Name: ', 'lastName'));

        $submitButton = new Submit('btnSave', 'Save');

        //add components to $form
        $form->addComponent($firstNameField)
             ->addComponent($lastNameField)
             ->addComponent($submitButton);

        //display form
        echo $form->display();
    }
}
```

The above code prints the following result

```html
<form action="http://localhost:8080/" method="post" accept-charset="utf-8">
    <label for="firstName">First Name: </label>
    <input type="text" name="firstname" value="">

    <label for="lastName">Last Name: </label>
    <input type="text" name="lastname" value="">

    <input type="submit" name="btnSave" value="Save">
</form>
```

Another example using more components

```php
<?php

namespace App\Controllers;

use CI4FormBuilder\Checkbox;
use CI4FormBuilder\Dropdown;
use CI4FormBuilder\Form;
use CI4FormBuilder\Input;
use CI4FormBuilder\Label;
use CI4FormBuilder\Password;
use CI4FormBuilder\Submit;

class Home extends BaseController
{
    public function index()
    {
        $form = new Form();

        //create an input to "firstname"
        $nameField = new Input('name'); 
        $nameField->setLabel(new Label('Name: ', 'name'));

        //create an input type email to "email"
        $emailField = new Input('email', '', '', 'email'); //last param is type of input
        $emailField->setLabel(new Label('Email: ', 'email'));

        $genderField = new Dropdown('gender', ['M' => 'Male', 'F' => 'Female']);
        $genderField->setLabel(new Label('Gender: ', 'gender'));

        $passField = new Password('password');
        $passField->setLabel(new Label('Password: ', 'password'));

        $checkTerms = new Checkbox('terms', 'Accept');
        $checkTerms->setLabel(new Label('Agree the terms', 'terms'));

        $submitButton = new Submit('btnSave', 'Save');

        //add components to $form (pass an array with all components)
        $form->addComponent([$nameField, $emailField, $genderField, $passField, $checkTerms, $submitButton]);

        //display form
        echo $form->display();
    }
}
```

Output:

```html
<form action="http://localhost:8080/" method="post" accept-charset="utf-8">

    <label for="name">Name: </label>
    <input type="text" name="name" value="">
	
    <label for="email">Email: </label>
    <input type="email" name="email" value="">
	
    <label for="gender">Gender: </label>
    <select name="gender">
        <option value="M">Male</option>
        <option value="F">Female</option>
    </select>
	
    <label for="password">Password: </label>
    <input type="password" name="password" value="">

    <label for="terms">Agree the terms</label>
    <input type="checkbox" name="terms" value="Accept">

    <input type="submit" name="btnSave" value="Save">
</form>
```
### Using a Template

With the `Template` class it is possible to define a default layout for a Form and its components. The following options are available for use

- `beforeForm` : code to insert before the `<form>` tag
- `afterForm` : code to insert after the `</form>` close tag
- `beforeComponent` : code to insert before each component
- `afterComponent` : code to insert after each component
- `beforeErrorMessage` : code to insert before a component error message (like validation) 
- `afterErrorMessage` : code to insert after a component error message

Example

```php
<?php

namespace App\Controllers;

use CI4FormBuilder\Form;
use CI4FormBuilder\Input;
use CI4FormBuilder\Label;
use CI4FormBuilder\Password;
use CI4FormBuilder\Submit;
use CI4FormBuilder\Template;

class Home extends BaseController
{
    public function index()
    {
        $form = new Form();

        //lets set the Template options
        $template = new Template([
            'beforeForm' => '<div class="form-login">',
            'afterForm' => '</div>',
            'beforeComponent' => '<div class="form-field">',
            'afterComponent' => '</div>',
        ]);

        //define the Form template
        $form->setTemplate($template);

        //create an input to "username"
        $usernameField = new Input('username'); 
        $usernameField->setLabel(new Label('Username: ', 'username'));

        //create an input to "password"
        $passField = new Password('password');
        $passField->setLabel(new Label('Password: ', 'password'));

        $submitButton = new Submit('btnLogin', 'Login');

        //add components to $form
        $form->addComponent([$usernameField, $passField, $submitButton]);

        //display form
        echo $form->display();
    }
}
```

Output:

```html
<div class="form-login">
	<form action="http://localhost:8080/" method="post" accept-charset="utf-8">

	<div class="form-field">
		<label for="username">Username: </label>
		<input type="text" name="username" value="">
	</div>

	<div class="form-field">
		<label for="password">Password: </label>
		<input type="password" name="password" value="">
	</div>

    <div class="form-field">
		<input type="submit" name="btnLogin" value="Login">
	</div>
</form>
</div>
```