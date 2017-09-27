# CustomFormに関する説明(サンプルコードはPHP)
ここに書いてあるコード、情報に誤りがあり、あなたにいかなる損害を与えたとしても私は責任を負わないものとします。 
2017/09/27 wakame0731

typeにcustom_formを指定すると送信ボタンが必ず追加されるのでjsonでは追加しなくてもよいです。  
ここでは以下のような画像のフォームを表示するjsonを書きます。
![カスタムフォームの画像](https://github.com/wakame0731/markdownTraining/blob/master/2017-09-27%20(11).png "custom_form")

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
これを送信するコードがこれだとします(適当だけど) 
```php
use pocketmine\network\mcpe\protocol\ModalFormRequestPacket;
use pocketmine\Player;

function createWindow(Player $player, $data, int $id){
    $pk = new ModalFormRequestPacket();
    $pk->formId = $id;//任意の数字
    $pk->formData = $data;//json
    $player->dataPacket($pk);
}
```
formIdを1とすると送信ボタンが押されたときのコードがこれになります(同じく適当)
```php
use pocketmine\event\server\DataPacketReceiveEvent;
use pocketmine\network\mcpe\protocol\ModalFormResponsePacket;
use pocketmine\Player;
function GetForm(DataPacketReceiveEvent $e){
    $p = $e->getPlayer();
    $pk = $e->getPacket();
    if ($pk instanceof ModalFormResponsePacket) {
        $id = $pk->formId;
        $data = $pk->formData;
        switch($id){
            case 1:
                if($data===null){
                    break;
                }
                $Fdata=json_decode($data,true);
                $p->sendMessage($Fdata[1]);
            break;
        }
    }
}
```
今回のFormの場合、送信ボタンが押されたときに返ってくるFormDataの値は`[null,"Default"]`となります。  
このコードではまず始めにjsonの形で書かれたformDataの値を配列にしています。  
すると`$Fdata[1]`にはinputに入力された文字が入っています。まあ後は好きにしてください。   
また、送信ボタンが押されずにFormが閉じた場合はnullが返ってきますので例外処理を忘れないようにしてください。  
以上で説明を終わります。  

custom_Formで使えるUIパーツに関する紹介はここにあるので参考にしてみてください。 
https://github.com/xBeastMode/McpeModalFormsDocumentation

