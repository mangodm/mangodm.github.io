---
title: (EN) Clean Code | Naming things
author: mangodm
date: 2022-11-21 11:33:00 +0800
categories: [CS, clean-code]
tags: [en, udemy]
math: true
---

These are notes from the [Clean Code](https://www.udemy.com/course/writing-clean-code/) course by Udemy.

---

# 01. Naming: Assigning good names

## Why good names matter

- Names should be meaningful because:
  - It allows readers to understand the code without going through it in detail.
  ```jsx
  const user = new User();
  database.insert(user);
  if (isLoggedIn) {...}
  ```

## Choosing good names

- variables, constants
  - They are data containers. (e.g.) user input data, validation results, a list of products
  - Use nouns or short phrases with adjectives
  - (e.g.) `const userData = {...} const isValid = {...}`
- functions & methods
  - commands or calculated values (e.g.) send data to server, check if user input is valid
  - Use verbs or short phrases with adjectives
  - (e.g.) `sendData() inputIsValid()`
- classes
  - Use classes to create “things” (e.g.) a user, a product, a HTTP request body
  - Use nouns or short phrases with nouns
  - (e.g.) `class User {...} class RequestBody {...}`

## Name casing

- snake_case: `is_valid`, `send_response` (e.g. Python - variables, functions, methods)
- camelCase: `isValid`, `sendResponse` (e.g. Java, JavaScript - variables, functions, methods)
- PascalCase: `AdminRole`, `UserRepository` (e.g. Python, Java, JavaScript - classes)
- kebab-case: `<side-bar>` (e.g. HTML - custom HTML elements)

## Naming variables & properties (theory)

- If a value is an object,
  - Name it to describe the value (e.g. `user`, `database`)
  - Provide more details without introducing redundancy (e.g. `authenticatedUser`, `sqlDatabase`)
- If a value is a number or a string,
  - Name it to describe the value (e.g. `name`, `age`)
  - Provide more details without introducing redundancy (e.g. `firstName`)
- If value is a Boolean,
  - Name it to answer a true/false question (e.g. `isActive`, `loggedIn`)
  - Provide more details without introducing redundancy (e.g. `isActiveUser`, `loggedIn`)

## Naming variables & properties (examples)

- To store a user object (i.e. name, email, age)
  - u, data < userData, person < user, customer
- To store a user input validation result (i.e. true, false)
  - v, val < correct, validatedInput < isCorrect, isValid

## Naming functions & methods (theory)

- If a function performs an operation,
  - Name it to describe the operation. (e.g. `getUser(...)`, `response.send()`)
  - Provide more details without introducing redundancy (e.g. `getUserByEmail(...)`, `response.send()`)
- If a function computes a boolean,
  - Name it to answer a true/false question. (e.g. `isValid(...)`, `purchase.isPaid()`)
  - Provide more details without introducing redundancy (e.g. `emailIsValid()`, `purchase.isPaid()`)

## Naming functions & methods (examples)

- To save user data to a database,
  - process, handle < save, storeData < saveUser, user.store
- To describe a function to validate the user input,
  - process, save < validateSave, check < validate, isValid

## Naming classes (theory)

- Name it to describe the objects (e.g. `User`, `Product`)
- Provide more details without introducing redundancy (e.g. `Customer`, `Course`)
- Avoid redundant suffixes (e.g. `DatabaseManager`

## Naming classes (example)

- To describe a user object,
  - class UEntity, ObjA < class UserObj, AppUser < class User, Admin
- To describe a database,
  - class Data, DataStorage < class Db, Data < class Database, SQLDatabase

## Common errors & pitfalls

- Don’t include redundant information in names
  - (e.g.) `userWithNameAndAge = User("Max", 31)` < `user = User("Max", 31)`
- Avoid slang, unclear abbreviations & disinformation
  - Slang
    - `product.diePlease()` < `product.remove()`
    - `user.facePalm()` < `user.sendErrorMessage()`
  - Unclear abbreviations
    - `message(n)` < `message(newUser)`
    - `ymdt = "20210121CET` < `dateWithTimeZone =`20210121CET`
  - Disinformation
    - `userList = {u1: ... u2: ...}` < `userMap = {u1: ... u2: ...}`
    - `allAccounts = accounts.filter()` < `filteredAccounts = accounts.filter()`
- Choose distinctive names
- Be consistent
  - `getUsers()`, `fetchUsers`, `retrieveUsers()`
  - You can go with either of those options, but stick with it throughout your entire program.

## Demo

- bad name examples

  ```python
  from datetime import datetime

  class Entity:
      def __init__(self, title, description, ymdhm):
          self.title = title
          self.description = description
          self.ymdhm = ymdhm

  def output(item):
      print('Title: ' + item.title)
      print('Description: ' + item.description)
      print('Published: ' + item.ymdhm)

  summary = 'Clean Code Is Great!'
  desc = 'Actually, writing Clean Code can be pretty fun. You\'ll see!'
  new_date = datetime.now()
  publish = new_date.strftime('%Y-%m-%d %H:%M')

  item = Entity(summary, desc, publish)

  output(item)
  ```

- clean name examples

  ```python
  from datetime import datetime

  class BlogPost:
  	def __init__(self, title, description, date_published):
  		self.title = title
  		self.description = description
  		self.date_published = date_published

  def print(self):
  	  print('Title: ' + self.title)
      print('Description: ' + self.description)
      print('Published: ' + self.date_published)

  title = 'Clean Code Is Great!'
  description = 'Actually, writing Clean Code can be pretty fun. You\'ll see!'
  now = datetime.now()
  formatted_date = now.strftime('%Y-%m-%d %H:%M')

  blog_post = BlogPost(title, description, formatted_date)

  blog_post.print()
  ```
