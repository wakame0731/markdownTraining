# CustomFormのjsonに関する説明
typeにcustom_formを指定すると送信ボタンが必ず追加されるのでjsonでは追加しなくてもよいです。 

```json
{
    "type": "custom_form",
    "title": "タイトル",
    "content": [
        {
            "type": "label",
            "text": "テキストはこれで書く"
        },
        {
            "type": "input",
            "text": "inputText",
            "placeholder": "placeholder",
            "default": "Default"
        }
    ]
}
```
