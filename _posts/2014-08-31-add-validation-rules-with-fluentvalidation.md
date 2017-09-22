---
id: 1418
title: Add Validation rules with FluentValidation
date: 2014-08-31T00:19:19+00:00
author: tiagosalgado
layout: post
guid: http://blog.tiagosalgado.com/?p=1418
slug: /2014/08/31/add-validation-rules-with-fluentvalidation/
dsq_thread_id:
  - 2973782806
categories:
  - dev
tags:
  - .net
  - 'C#'
  - FluentValidation
---
One scenario we have in almost all applications where requires user inputs, are validation rules. We have lot of different ways to implement validation (both server and client sides), and one of these ways is using a library called <a href="https://fluentvalidation.codeplex.com" target="_blank">FluentValidation</a>, where it uses lambda expressions to build all validation rules.

To integrate it, we can get the package from Nuget and add it to our project:

![fluentvalidation nuget](/content/nuget.png){:class="img-responsive"}

This will add FluentValidation references.

For this example, we&#8217;ll use a simple model such as:

<pre class="brush: csharp; title: ; notranslate" title="">public class Person
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public int Age { get; set; }
}
</pre>

Now, to create our validator, we need to have a class who inherits from AbstractValidator<T>, where T in this case will the Person.

<pre class="brush: csharp; title: ; notranslate" title="">using FluentValidation;
using FluentValidationExample.Models;
namespace FluentValidationExample.Validators
{
    public class PersonValidator : AbstractValidator&lt;Person&gt;
    {
    }
}
</pre>

Having our class created, we can now define the rules to validate a Person entity in the validator class constructor. For this example, I&#8217;ll add two rules, one to validate if the first name is not empty and other to make sure we have a age greater than 18.

<pre class="brush: csharp; title: ; notranslate" title="">public PersonValidator()
{
    RuleFor(d =&gt; d.FirstName)
        .NotEmpty()
        .WithMessage("First name cannot be empty");

    RuleFor(d =&gt; d.Age)
        .Must(AgeMustBeGreaterThan18)
        .WithMessage("Age must be greater than 18");
}

private bool AgeMustBeGreaterThan18(int age)
{
    return age &gt; 18;
}
</pre>

FluentValidation already includes some built-in validations, so for our first validation we can use the NotEmpty() method. For the second validation, we&#8217;ve created a new method who verifies the Age property value and makes sure is greater than 18. If not, it will throw an error.
  
In this case, the validator will go through all rules and will send the result back with any errors, but we can decide to stop on the first failure.

<pre class="brush: csharp; title: ; notranslate" title="">public PersonValidator()
{
    RuleFor(d =&gt; d.FirstName)
        .Cascade(CascadeMode.StopOnFirstFailure)
        .NotEmpty()
        .WithMessage("First name cannot be empty");

    RuleFor(d =&gt; d.Age)
        .Must(AgeMustBeGreaterThan18)
        .WithMessage("Age must be greater than 18");
}

private bool AgeMustBeGreaterThan18(int age)
{
    return age &gt; 18;
}
</pre>

Having the validator created, we now need to call it when we want to validate all rules for the Person model, check if there are any errors, and display (and log) those to the user.

<pre class="brush: csharp; title: ; notranslate" title="">static void Main(string[] args)
{
    var person = new Person
    {
        FirstName = "",
        LastName = "Salgado",
        Age = 17, // I wish
        Address = "London, UK"
    };

    var validator = new PersonValidator();

    var validationResult = validator.Validate(person);

    if (!validationResult.IsValid)
    {
        validationResult.Errors.ToList().ForEach(p =&gt; Console.WriteLine(p.ErrorMessage));
    }

    Console.Read();
}
</pre>

I&#8217;m not defining the FirstName and i&#8217;ve set the age as 17 to force both rules to fail, so the output will be both error messages:

![console output](/content/console_output.png){:class="img-responsive"}

This is just a example of how we can add FluentValidation to a project, but much more advanced validations can be done, so I recommend you to read the <a href="https://fluentvalidation.codeplex.com/documentation" target="_blank">documentation </a>and follow the examples available in the project page.