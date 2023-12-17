# Eclipse

> Tweette detaylar mevcuttu.

> İşlemleri herhangi bir sunucumuzda yapalım muhim değil.


## Contrat Deploy:

```console
# altta ki curl ile başlayan komutu girdikten sonra 1 i seçelim:
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

# solana cli kurulumu - komutları tek tek kullanalım.
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
PATH="/root/.local/share/solana/install/active_release/bin:$PATH"
solana config set --url https://staging-rpc.dev.eclipsenetwork.xyz

# bu komut sonrası => key oluşacak, şifre belirleyip mnemonicleri ve cüzdan adresini kaydediyoruz
solana-keygen new

# bu komutta cüzdana token alıyoruz
solana airdrop 10

# bu komut çalışması için 8 ram lazım - github swap space repom ile çözersiniz.
solana-test-validator
# komut çalışınca ctrl c ile durdurabilirsiniz.

# 5 gün sonra not - 8 RAM ile dahi çözülmediyse farklı bir durum söz konusu - chatte yardım isteyiniz.

# nodejs kurulumu - komutları tek tek kullanalım
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y

# kontrat deploy işlemini gerçekleştirelim
git clone https://github.com/solana-labs/example-helloworld
cd example-helloworld
npm install

# burda biraz bekletecek rusttan dolayı - uzun sürebilir.
npm run build:program-rust

# program id not edin - FORMDA SONRADAN BU ID KULLANACAKSINIZ
solana program deploy dist/program/helloworld.so
npm run start
# success çıktısı verecek en sonda.

```

## Bridge işlemi:

```console
sudo apt update -y && sudo apt upgrade -y
sudo apt install git

git clone https://github.com/Eclipse-Laboratories-Inc/testnet-deposit.git
cd testnet-deposit

yarn install
yarn add ethers

# Altta ki komutu düzenleyelim (tırnakları kaldırın):
node deposit.js <solanaAdresi> 0x7C9e161ebe55000a3220F972058Fb83273653a6e 500000 100 <MetamaskPrivateKeyi> https://rpc.sepolia.org
# solana cüzdanı yukarıda oluşturduk onu import edin yeni profilde.
```

> başarılıysa success ve tx çıktısı verecek

> https://explorer.dev.eclipsenetwork.xyz/?cluster=testnet burdan solana cüzdanı kontrol edip eth var mı yok mu bakıyoruz. varsa ok.

> Formu doldurun: https://docs.google.com/forms/d/e/1FAIpQLSfJQCFBKHpiy2HVw9lTjCj7k0BqNKnP6G1cd0YdKhaPLWD-AA/viewform
