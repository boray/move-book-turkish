# Getting started

> **Warning:** Content on this page is outdated and requires rework. Newer version of Move IDE will be published soon. For now I recommend you to use [move-cli](https://github.com/diem/diem/tree/main/language/tools/move-cli).

---

Herhangi bir derlenmiş dilde olduğu gibi, Move uygulamalarınızı derlemek, çalıştırmak ve hatalarını ayıklamak için uygun bir araç setine ihtiyacınız vardır. Bu dil blok zincirleri için oluşturulduğundan ve yalnızca bunların içinde kullanıldığından, komut dosyalarını zincir dışı çalıştırmak önemsiz bir görevdir: her modül bir ortam, hesap işleme ve derleme-yayınlama sistemi gerektirir.

Move modüllerinin geliştirilmesini basitleştirmek için Visual Studio Code için [Move IDE](https://github.com/damirka/vscode-move-ide) uzantısını oluşturdum. Bu uzantı, ortam gereksinimleriyle başa çıkmanıza yardımcı olacaktır. Bu uzantının kullanılması, sizin için inşa/çalıştır ortamını idare edeceğinden şiddetle tavsiye edilir, bu nedenle komut satırı arayüzü ile uğraşmak yerine Move dilini öğrenmeye odaklanmanıza izin verir. Bu uzantı ayrıca, uygulamalarınızın genel kullanıma sunulmadan önce hatalarının ayıklanmasına yardımcı olmak için sözdizimi vurgulamayı ve yürütücüyü Taşı içerir.

## Move IDE Kurulumu

Yüklemek için aşağıdakilere ihtiyacınız olacak:

1. VSCode (versiyon 1.43.0 ve üstü) - [buradan indirebilirsin](https://code.visualstudio.com/download); eğer zaten kuruluysa sonraki adıma geçin;
2. Move IDE - VSCode yüklendikten sonra, [buradan indirebilirsiniz](https://marketplace.visualstudio.com/items?itemName=damirka.move-ide) IDE'nin en güncel versiyonunu kullandığınızdan emin olun.
   
### Ortamı Kur

Move IDE, dizin yapınızı düzenlemenin tek bir yol önerir. Projeniz için yeni bir dizin oluşturun ve VSCode'da açın. Ardından bu dizin yapısını kurun:

```
modules/   - modüller için dizin
scripts/   - işlem(transaction) scriptleri dosyaları için dizin
out/       - bu dizin derlenmiş kaynak dosyaları tutacak
```

Ayrıca, çalışma ortamınızı yapılandıracak olan `.mvconfig.json` adlı bir dosya oluşturmanız gerekecektir. Bu, "libra" için bir örnektir:

```json
{
    "network": "libra",
    "sender": "0x1"
}
```

Alternatif olarak, ağ olarak "dfinance" kullanabilirsiniz:

```json
{
    "network": "dfinance",
    "sender": "0x1"
}
```

> dfinance bech32 'wallet1...' adreslerini, libra ise 16 baytlık '0x...' adreslerini kullanır. Yerel çalıştırmalar ve deneyler için 0x1 adresi yeterlidir - basit ve kısadır. Testnet veya production ortamında gerçek blok zincirlerle çalışırken, seçtiğiniz ağın doğru adresini kullanmanız gerekecek.

## Move ile İlk Uygulama

Move IDE, scriptleri bir test ortamında çalıştırmanıza olanak tanır. `gimme_five()` fonksiyonunu uygulayarak ve VSCode içinde çalıştırarak nasıl çalıştığını görelim.

### Modül Oluştur

Projenizin 'modules/' dizini içinde 'hello_world.move' adında yeni bir dosya oluşturun.
```Move
// modules/hello_world.move
address 0x1 {
module HelloWorld {
    public fun gimme_five(): u8 {
        5
    }
}
}
```

> Kendi adresinizi kullanmaya karar verdiyseniz ("0x1" yerine) - bu dosyada ve aşağıdaki dosyada 0x1'i değiştirdiğinizden emin olun.

### Script Yaz

Sonra bir script oluşturun, buna "scripts/" dizini içinde "me.move" diyelim:
```Move
// scripts/run_hello.move
script {
    use 0x1::HelloWorld;
    use 0x1::Debug;

    fun main() {
        let five = HelloWorld::gimme_five();

        Debug::print<u8>(&five);
    }
}
```

Ardından, scriptinizi açık tutarken şu adımları izleyin:
1. '⌘+Shift+P' (Mac'te) veya 'Ctrl+Shift+P' (Linux/Windows'ta) tuşlarına basarak VSCode'un komut paletini değiştirin
2. `>Move: Run Script` yazın ve doğru seçeneği gördüğünüzde enter'a basın veya tıklayın.

Voila! You should see the execution result - log message with '5' printed in debug. If you don't see this window, go through this part again.
İşte! Execution(?) sonucunu görmelisiniz - hata ayıklamada '5' yazdırılmış log mesajı. Bu pencereyi görmüyorsanız, bu bölümü tekrar gözden geçirin.

Dizin yapınız şöyle görünmelidir:
```
modules/
  hello_world.move
scripts/
  run_hello.move
out/
.mvconfig.json
```

> Modüller dizininde istediğiniz kadar modül bulundurabilirsiniz; hepsine .mvconfig.json'da belirttiğiniz adres altında komut dosyalarınızda erişilebilir olacaktır.