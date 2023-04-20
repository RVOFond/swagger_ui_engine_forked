# * 
This is a copy of a fork by another developer. A currently maintained project was depending on this fork and when the original repo was removed, we needed to resolve the dependency. We did so by recovering a local copy of the fork and publishing it here. 

# _Fork note!_

This is a fork with the goal of deleting the version dependency on rails version lower than 6.0. Instead the dependency is the other way around, you will need a higher version than 6.0
No other changes than those required for this modernization have been introduced.

# SwaggerUiEngine

Include [swagger-ui](https://github.com/swagger-api/swagger-ui) as Rails engine and document your API with simple YAML files.

## Versions

| Swagger UI version | Rails version |
| ------------------ | ------------- |
| 2.2.10             | >= 6          |

## Features

- Supports API documentation versioning / multiple APIs documentation
- Easily configurable Swagger interface and OAuth2 authorization
- Enables using Swagger interface translations
- Works with API-only applications
- Simple, clear and actively maintained :ok_woman:

## Installation

Add to Gemfile

```
gem 'swagger_ui_engine', :git => git@github.com:krzykamil/swagger_ui_engine.git
```

And then run:

```
$ bundle
```

## Usage

### Mount

Add to your `config/routes.rb`

```
mount SwaggerUiEngine::Engine, at: "/api_docs"
```

You can place this route under `admin_constraint` or other restricted path, or configure basic HTTP authentication.

#### Devise auth

```
authenticate :user, lambda { |u| u.admin? } do
  mount SwaggerUiEngine::Engine, at: "/api_docs"
end
```

#### Basic HTTP auth

Set admin username and password in an initializer:

```
# config/initializers/swagger_ui_engine.rb

SwaggerUiEngine.configure do |config|
  config.admin_username = ENV['ADMIN_USERNAME']
  config.admin_password = ENV['ADMIN_PASSWORD']
end
```

### Initialize

#### Versioned API documentations

Set the path of your json/yaml versioned documentations in an initializer:

```
# config/initializers/swagger_ui_engine.rb

SwaggerUiEngine.configure do |config|
  config.swagger_url = {
    v1: 'api/v1/swagger.yaml',
    v2: 'api/v2/swagger.yaml',
  }
end
```

and place your main documentation file under `/public/api` path.

#### Single API documentation

**NOTE**: This is a compatibility patch for the `SwaggerUiEngine` gem versions `<= 0.0.5`. It's recommended to version your API documentation from the beginning.

You can define your main documentation url in a hash value (same way as in the versioned documentations) or pass single string with the url:

```
# config/initializers/swagger_ui_engine.rb

SwaggerUiEngine.configure do |config|
  config.swagger_url = 'api/swagger.yaml'
end
```

### Configure

| Config Name               | Swagger parameter name | Description                                                                                                                                                                                                                                                            |
| ------------------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config.swagger_url        | url                    | The url pointing `swagger.yaml` (Swagger 2.0) as per [OpenAPI Spec](https://github.com/OAI/OpenAPI-Specification/). Accepts a Hash, with API version names as keys and documentation URLs as values. Also accepts a String pointing to documentation for all versions. |
| config.doc_expansion      | docExpansion           | Controls how the API listing is displayed. It can be set to 'none' (default), 'list' (shows operations for each resource), or 'full' (fully expanded: shows operations and their details).                                                                             |
| config.model_rendering    | defaultModelRendering  | Controls how models are shown when the API is first rendered. It can be set to 'model' or 'schema', and the default is 'schema'.                                                                                                                                       |
| config.request_headers    | showRequestHeaders     | Whether or not to show the headers that were sent when making a request via the 'Try it out!' option. Defaults to `false`.                                                                                                                                             |
| config.json_editor        | jsonEditor             | Enables a graphical view for editing complex bodies. Defaults to `false`.                                                                                                                                                                                              |
| config.translator_enabled | translations           | Enables Swagger Ui translations. Defaults to `false`.                                                                                                                                                                                                                  |
| config.validator_enabled  | validatorUrl           | Enables documentation validator. Defaults to `false` (`validatorUrl: 'null'`).                                                                                                                                                                                         |

#### OAuth2 configuration

You can configure OAuth2 default authorization.

| Config Name                      | Swagger parameter name      | Description                                                                                                                  |
| -------------------------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| config.oauth_client_id           | client_id                   | Default clientId. MUST be a string                                                                                           |
| config.oauth_client_secret       | client_secret               | Default clientSecret. MUST be a string                                                                                       |
| config.oauth_realm               | realm                       | realm query parameter (for oauth1) added to `authorizationUrl` and `tokenUrl` . MUST be a string                             |
| config.oauth_app_name            | appName                     | application name, displayed in authorization popup. MUST be a string                                                         |
| config.oauth_scope_separator     | scopeSeparator              | scope separator for passing scopes, encoded before calling, default value is a space (encoded value `%20`). MUST be a string |
| config.oauth_query_string_params | additionalQueryStringParams | Additional query parameters added to `authorizationUrl` and `tokenUrl`. MUST be an object                                    |

### Edit your json/yaml files

You can use [Swagger editor](https://github.com/swagger-api/swagger-editor) to write and validate your Swagger docs.

## Example app

Here's an example use of [SwaggerUiEngine in Rails application](https://github.com/zuzannast/swagger_ui_engine_example).

## License

This project is available under MIT-LICENSE. :sunny:
