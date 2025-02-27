1. Перейдите в директорию `~/.aws/` (для macOS и Linux) или `C:\Users\<имя_пользователя>\.aws\` (для Windows).
1. Создайте файл `credentials` с аутентификационными данными для {{ objstorage-name }} и скопируйте в него следующую информацию:

    ```text
    [default]
      aws_access_key_id = <идентификатор_статического_ключа>
      aws_secret_access_key = <секретный_ключ>
    ```

1. Создайте файл `config` с параметрами региона по умолчанию и скопируйте в него следующую информацию:

    ```text
    [default]
      region={{ region-id }}
      endpoint_url=https://{{ s3-storage-host }}
    ```

    {% note info %}

    Некоторые приложения, предназначенные для работы с Amazon S3, не позволяют указывать регион, поэтому {{ objstorage-name }} принимает также значение `us-east-1`.

    {% endnote %}