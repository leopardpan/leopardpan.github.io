---
layout: post
title: "使用chatGPT学习BDD接口测试"
date: 2023-03-17
description: "介绍了基于chatGPT学习行为驱动开发接口自动化测试"
tag: 自动化测试
---

## BDD接口测试是什么？

BDD (Behavior Driven Development) 是一种软件开发方法，旨在通过定义和执行行为来增强软件的可理解性和可维护性。在接口测试中，BDD 可以帮助我们更好地描述接口的行为和期望结果，从而提高测试用例的可读性和可维护性。

Pytest-BDD 是一个 Pytest 插件，它提供了对 BDD 测试的支持。下面是一些使用 Pytest-BDD 进行接口测试的步骤：

1. 安装 Pytest-BDD

可以使用 pip 命令安装 Pytest-BDD：

```
pip install pytest-bdd
```

2. 定义 Feature 文件

在 BDD 中，测试用例通常以 Feature 文件的形式进行组织。Feature 文件是一个文本文件，用于描述测试用例的场景和期望结果。例如，以下是一个示例 Feature 文件：

```gherkin
Feature: User API

Scenario: Create a new user
    Given I have a valid access token
    When I make a POST request to "/users" with the following data
        | username | john_doe |
        | email    | john_doe@example.com |
        | password | 123456 |
    Then the response status code should be 201
    And the response should contain the following data
        | username | john_doe |
        | email    | john_doe@example.com |
```

在这个示例中，Feature 文件定义了一个名为 "User API" 的 Feature，其中包含一个名为 "Create a new user" 的 Scenario。Scenario 中描述了测试用例的场景和期望结果，包括给定的前提条件 (Given)、操作 (When) 和断言 (Then)。

3. 定义 Step Definitions

Step Definitions 是一个 [Python]() 模块，用于实现 Feature 文件中定义的 Given、When 和 Then 步骤。例如，以下是一个示例 Step Definitions：

```python
import requests
from pytest_bdd import given, when, then, parsers

@given("I have a valid access token")
def valid_access_token():
    # 返回一个有效的访问令牌
    return "valid_token"

@when(parsers.parse('I make a {method} request to "{url}" with the following data'))
def make_request(method, url):
    # 根据方法和 URL 发送请求
    # 将数据作为请求正文发送
    response = requests.request(method, url, json=parsers.parse_table())
    return response

@then(parsers.parse('the response status code should be {status_code:d}'))
def check_status_code(make_request, status_code):
    assert make_request.status_code == status_code

@then(parsers.parse('the response should contain the following data'))
def check_response_data(make_request):
    expected_data = parsers.parse_table()
    actual_data = make_request.json()
    assert expected_data in actual_data.values()
```

在这个示例中，Step Definitions 实现了 Feature 文件中定义的 Given、When 和 Then 步骤。[Step Definitions]() 使用 Pytest-BDD 提供的装饰器 (如 @given、@when 和 @then) 来标记步骤，以及 Pytest-BDD 提供的 parsers.parse 方法来解析表格数据。

4. 运行测试用例

使用 Pytest 运行测试用例时，可以使用 `--bdd` 参数来指定测试目录和模式，如：

```
pytest --bdd tests/ --verbose
```

在运行测试用例时，Pytest-BDD 将自动扫描测试目录下的 Feature 文件和 Step Definitions，然后根据 Feature 文件定义的场景执行测试用例。

总之，使用 Pytest-BDD 进行接口测试可以帮助我们更好地描述接口的行为和期望结果，从而提高测试用例的可读性和可维护性。建议了解 Pytest-BDD 官方文档中有关 BDD 测试的更多内容，以便更好地使用 Pytest-BDD 进行接口测试。

## 定义 Feature 文件的格式可以展开说明一下吗

Feature 文件是 BDD 测试中用于描述测试用例的场景和期望结果的文本文件。它通常采用 Gherkin 语言进行编写，Gherkin 语言是一种基于自然语言的 [DSL]() (Domain Specific Language)，它提供了一种简单易懂的语法，用于描述测试用例的场景和期望结果。

下面是一个 Feature 文件的示例，它描述了一个用户管理系统的测试用例场景：

```gherkin
Feature: User Management
    As a system administrator
    I want to manage users
    So that I can manage access to the system

    Scenario: Create a new user
        Given I am logged in as an administrator
        When I create a new user with the following details
            | username | john_doe |
            | email    | john_doe@example.com |
            | password | 123456 |
        Then the user should be created successfully

    Scenario: Update an existing user
        Given I am logged in as an administrator
        And there is an existing user with the username "jane_doe"
        When I update the user with the username "jane_doe" with the following details
            | email    | jane_doe@example.com |
            | password | 654321 |
        Then the user should be updated successfully

    Scenario: Delete an existing user
        Given I am logged in as an administrator
        And there is an existing user with the username "jane_doe"
        When I delete the user with the username "jane_doe"
        Then the user should be deleted successfully
```

在这个示例中，Feature 文件包含了一个名为 "User Management" 的 Feature，它描述了一个用户管理系统的测试用例场景。Feature 文件中的每个 Scenario 描述了一个测试用例，包括前提条件 (Given)、操作 (When) 和断言 (Then)。

Feature 文件的基本语法格式如下：

```
Feature: <feature name>
    <feature description>

    Scenario: <scenario name>
        Given <precondition>
        When <action>
        Then <assertion>

    Scenario: <scenario name>
        Given <precondition>
        When <action>
        Then <assertion>

    ...
```

其中，Feature、Scenario、Given、When 和 Then 等关键字都是 Gherkin 语言中的保留关键字，用于描述测试用例的场景和期望结果。Feature 文件的详细格式和语法可以参考 Gherkin 语言的官方文档和 Pytest-BDD 的官方文档。


## Step Definitions中的@when(parsers.parse('I make a request to "" with the following data'))我看不太懂

这段 Step Definitions 中的 `@when(parsers.parse('I make a request to "{url}" with the following data'))` 是一个装饰器，用于标记一个 When 步骤。该步骤对应 Feature 文件中的一行，例如：

```gherkin
When I make a request to "https://example.com/api/users" with the following data
```

装饰器的参数 `parsers.parse('I make a request to "{url}" with the following data')` 表示使用 `parsers.parse()` 方法来解析 Feature 文件中的文本，并将其中的 `{url}` 参数解析为 Step Definitions 函数的 `url` 参数。

当 Pytest-BDD 执行测试用例时，它会将 Feature 文件中的文本传递给 Step Definitions 函数，例如：

```python
@when(parsers.parse('I make a request to "{url}" with the following data'))
def make_request(url):
    # 执行 HTTP 请求，使用 url 和数据表格中的数据
    pass
```

在这个示例中，`make_request()` 函数接受一个 `url` 参数，并且使用该参数和数据表格中的数据来执行 HTTP 请求。例如，如果 Feature 文件中的文本是 `When I make a request to "https://example.com/api/users" with the following data`，则 Pytest-BDD 会将 `https://example.com/api/users` 作为 `url` 参数传递给 `make_request()` 函数，并且将数据表格作为函数的参数传递。

希望这样的解释能够帮助您理解 Step Definitions 中的装饰器和参数。


## 我看不太懂@then(parsers.parse('the response status code should be')) def check_status_code(make_request, status_code): assert make_request.status_code == status_code

这段代码是一个 [Step Definitions]() 中的 `@then` 装饰器，用于标记一个 Then 步骤。该步骤对应于 Feature 文件中的一行，例如：

```gherkin
Then the response status code should be 200
```

装饰器的参数 `parsers.parse('the response status code should be {status_code:d}')` 表示使用 `parsers.parse()` 方法来解析 Feature 文件中的文本，并将其中的 `{status_code}` 参数解析为 Step Definitions 函数的 `status_code` 参数。`d` 表示将该参数解析为整数类型。

当 Pytest-BDD 执行测试用例时，它会将 Feature 文件中的文本传递给 Step Definitions 函数，例如：

```python
@then(parsers.parse('the response status code should be {status_code:d}'))
def check_status_code(make_request, status_code):
    assert make_request.status_code == status_code
```

在这个示例中，`check_status_code()` 函数接受一个 `make_request` 参数和一个 `status_code` 参数。`make_request` 参数是一个在之前的 Given 或 When 步骤中执行的请求对象（例如使用 requests 库发出的 HTTP 请求），而 `status_code` 参数则是从 Feature 文件中解析出来的 HTTP 状态码。

该函数的实现是检查 `make_request` 对象的状态码是否等于 `status_code` 参数。如果状态码不相等，那么该断言会失败，从而标记该测试用例为失败。

希望这样的解释能够帮助您理解 Step Definitions 中的装饰器和参数，并理解其在 Pytest-BDD 中的作用。


## BDD接口自动化测试怎么管理很多的测试数据呢


在 BDD 接口自动化测试中，管理大量测试数据的方法有很多种，具体根据项目的实际情况和需求来选择。

以下是一些常用的管理测试数据的方法：

1. 数据库管理：将测试数据存储在数据库中，并使用 SQL 语句来获取和操作数据。这种方法可以方便地进行数据的增删改查和管理，但需要一些数据库知识和维护成本。
2. Excel/CSV 文件管理：将测试数据存储在 [Excel]() 或 [CSV]() 文件中，使用 [Python]() 的 pandas 库或其他数据处理库来读取和操作数据。这种方法可以方便地进行数据的导入导出和管理，但需要一些额外的开发工作。
3. 配置文件管理：将测试数据存储在配置文件中，例如 [YAML]() 或 [JSON]() 格式的文件，使用 Python 的 [ConfigParser]() 或其他配置文件解析库来读取和操作数据。这种方法可以方便地进行数据的配置和管理，但需要一些额外的开发工作。
4. 数据生成工具：使用 Python 的 Faker 库或其他数据生成库来生成测试数据。这种方法可以方便地生成大量的测试数据，但需要确保生成的数据符合业务逻辑和要求。

无论采用哪种方法，都需要注意测试数据的可维护性和可重用性，以便在测试用例变更或新增时能够快速地修改或添加测试数据。同时，也需要对测试数据进行合理的组织和分类，以便于测试用例的管理和维护。

## 可以更加具体说一下这个步骤吗，还是不懂怎么写入大数据到feature中


如果您需要在 Feature 文件中写入大量的测试数据，可以使用数据表格来组织数据，并将数据表格写入 Feature 文件中。数据表格是 Feature 文件中以表格格式表示的数据，可以包含多行和多列的数据。

以下是一个示例 Feature 文件，其中包含一个数据表格：

```gherkin
Feature: 订单管理

  Scenario: 创建订单
    Given 用户登录系统
    When 用户创建订单
      | 商品名称 | 数量 |
      | 商品A    | 2    |
      | 商品B    | 1    |
    Then 订单创建成功
```

在上面的示例中，数据表格位于 `When` 步骤下面，使用管道符 `|` 来分隔每一列的数据。第一行表示每一列的列名，后面的行表示实际的数据。您可以根据需要添加或删除列和行，以适应不同的测试场景。

[在 Step Definitions]() 中，您可以使用 Pytest-BDD 提供的 `parsers.parse_table()` 方法来解析数据表格，并将其转换为 Python 中的数据结构，例如列表或字典。以下是一个示例 [Step Definitions]()：

```python
@when(parsers.parse('用户创建订单'))
def create_order():
    pass

@when(parsers.parse('用户创建订单'))
def create_order(make_request, data):
    for row in data:
        product_name = row['商品名称']
        quantity = row['数量']
        # 使用商品名称和数量创建订单项
        pass
```

在上面的示例中，`create_order()` 函数接受一个 `make_request` 参数和一个 `data` 参数，其中 `data` 参数是通过 `parsers.parse_table()` 方法解析出来的数据表格。在函数中，我们可以遍历数据表格的每一行，并使用其中的数据来执行一系列操作，例如创建订单项。

通过使用数据表格和 `parsers.parse_table()` 方法，您可以方便地将大量测试数据写入 Feature 文件和 Step Definitions 中，并且可以轻松地扩展和维护这些数据。
