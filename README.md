# ⚙️ LangChain Kurulumu 🦜🔗

## 🎯 Bu projenin amacı

LLM uygulamalarımız için ayrı bir ortam kurulumu.

Bu ünitede birçok yeni pakete ihtiyacımız var. Bu paketlerin bazıları, bootcamp'in diğer bölümleri için ihtiyaç duyduğumuz paket sürümleriyle uyumlu değil.

Bu nedenle yeni bir sanal ortam kuracağız. Bu şekilde iki ayrı python ortamımız olacak ve paketlerimiz çakışmayacak. 🦾

Bu, projeleriniz için sıklıkla yapacağınız bir şey: özel sanal ortamlar oluşturmak.

## 🐍 Yeni bir sanal ortam oluşturun

Bu adımlardan herhangi biri başarısız olursa, yardım için bir eğitmenden destek alınız.

👣 Proje klasöründe olduğunuzdan emin olun:

👣 Ünitenin ana klasörüne gidin:

```bash
cd ..
```

🐍 Python sürümünüzü kontrol edin. `3.12.9` gibi bir çıktı vermelidir:

```bash
python --version
```

🐍 Sanal ortamı oluşturun (aşağıdaki `<YOUR_PYTHON_VERSION>` kısmını önceki komutun çıktısıyla değiştirin (örn. `3.12.9`))

```bash
pyenv virtualenv <YOUR_PYTHON_VERSION> langchain-env
```

🐍 Bu ünitedeki tüm meydan okumaların bu sanal ortamı kullanması için sanal ortamı yerel olarak etkinleştirin:

```bash
pyenv local langchain-env
```

✅ Terminalinizin sağ tarafında `[🐍 langchain-env]` görüntülediğinden emin olun.


⚙️ Son olarak, pip'i güncelleyin (pip paket yükleyicisidir):

```bash
pip install --upgrade pip
```


## 📦 Paketleri yükleyin

Bu projenin klasörüne geri dönün:

```bash
cd ~/code/<user.github_nickname>/{{local_path_to("06-Deep-Learning/07-GenAI-and-RAG/00-LangChain-Env")}}
```

Bu klasörde, bu ünitenin projeleri için tüm gereksinimleri içeren bir `requirements.txt` dosyası oluşturduk. Hepsini kurmak için sadece `pip install` yapmamız gerekiyor:

```bash
pip install -r requirements.txt
```

Aslında şunları yüklüyoruz:
- Jupyter Notebook ve tüm bağımlılıkları
- Pandas ve NumPy gibi klasikler
- Google'ın Gemini LLM'i ile doğrudan etkileşim kurmak için `google-genai`
- `langchain`
- LangChain aracılığıyla Gemini kullanabilmek için `langchain-google-genai`
- Ekstra araçlarla birlikte `langchain-community`
- Bir vektör deposu olan `langchain-chroma`
- PDF dosyalarını yüklemek için `pypdf`

## 🔑 API aracılığıyla Gemini kullanmak için kimlik doğrulama

Bu ünitenin projede Google'ın amiral gemisi LLM'i olan Gemini'yi kullanacağız.

Google, Gemini'yi API aracılığıyla kullanmak için iki olasılık sunar:
- Bağımsız Gemini Developer API,
- Google Cloud Platform (GCP)'nin bir parçası olan Vertex AI Gemini API.

Bağımsız API, Gemini destekli uygulamalar oluşturmanın en hızlı yoludur. Eskiden cömert bir ücretsiz katmanı vardı, ancak bu ücretsiz katman Aralık 2025'te büyük ölçüde azaltıldı ve bu ünitede yapacaklarımız için yeterli değil.

Google Cloud'da zaten ücretsiz bir deneme hesabı oluşturdunuz, ya kurulum gününde  ya da yakın zamanda. Bu, GCP'nin Vertex AI Gemini API'sini kullanmamıza ve ücretsiz kredilerimizi kullanarak bunun için ödeme yapmamıza olanak tanıyacak.

### Projeler için Google Cloud kurulumu

Bu ünitenin projeleri için Google Cloud'u kuralım.

:warning: Bu, Google Cloud'u kurulum gününde veya yakın zamanda zaten kurduğunuzu varsayar. Kurulumun bu bölümünü yapıp yapmadığınızdan emin değilseniz, aşağıdaki talimatları izleyin. GCP kurulumunu yapmadıysanız, 4. adımda engelleneceksiniz. Bu durumda, yardım için bir eğitmene başvurun.

1. Bu projenin klasöründe olduğunuzdan emin olun:

2. `.env.vertex.sample` dosyasını bu ünitenin tüm projelerini içerecek klasörde yeni bir `.env` dosyasına kopyalayın:

    ```bash
    cp .env.vertex.sample ../.env
    ```

3. `.env` dosyasını favori kod editörünüzle açın:

    ```bash
    code ../.env
    ```

4. Terminalinize geri dönün ve bu komutu çalıştırın:

    ```bash
    gcloud config get project
    ```

    Çıktının ilk satırında, kurulum sırasında oluşturduğunuz projenin adını verecektir.

    🚫  Vermiyorsa, yardım için bir eğitmene başvurun. GCP kurulumunuzla ilgili muhtemelen bir sorun var. Bir eğitmene başvurun. 🚫


5. Daha önce oluşturduğunuz `.env` dosyasında, `your_project_id` kısmını projenizin adıyla (önceki adımın çıktısı) değiştirin.

6. Dosyayı kaydedin ve kapatın.

7. Son olarak, projenizde Vertex AI'yi etkinleştirelim. Terminalinizde aşağıdaki satırı çalıştırın:

    ```bash
    gcloud services enable aiplatform.googleapis.com
    ```

    `Operation "operations/......." finished successfully.` çıktısını vermelidir. Vermiyorsa, yardım için bir eğitmene başvurun.


Sonraki bölümü atlayın (sadece referans içindir) ve _Kurulumunuzu kontrol edin adımına_ geçin.


### 🔑 Google Gemini API anahtarı

🚧 Bu bölüm **sadece referans içindir**, bağımsız API'yi kullanmak istediğiniz durumlar için. Şimdilik bunu atlayın. 🚧

<details>
  <summary>Gemini Developer API için talimatlar
  </summary>

Gemini'yi Gemini Developer API aracılığıyla kullanmak için, Google'ın Gemini API'si için bir API anahtarı almak üzere kaydolmanız gerekir.

Başlayalım:

1. `https://aistudio.google.com/apikey` adresine gidin
2. Google hesabınızla henüz giriş yapmadıysanız, giriş yapın.
3. Sağ üst köşede, mavi `Create API key` düğmesine tıklayın.
4. API anahtarınızı oluşturmak için adımları takip edin. Unutmayın: faturalandırma ayarlamaya gerek yok, ücretsiz katımda kalacağız.

Kişisel API anahtarımızı doğrudan oluşturacağımız notebook'lar ve `.py` dosyalarına kaydetmek istemiyoruz. Şunu düşünün: kodumuzı daha sonra başkalarıyla paylaşmak isteyebiliriz, ancak onlar API anahtarlarımızı almamalı.

Bunun yerine, anahtarı ayrı bir `.env` dosyasına kaydedeceğiz. Daha sonra kütüphanelerin Gemini API'si ile kimlik doğrulaması yapması gerektiğinde kullanabilmesi için anahtarı bu dosyadan belleğe yükleyeceğiz.

Hadi yapalım:

1. Bu projenin klasöründe olduğunuzdan emin olun:

2. `.env.gemini.sample` dosyasını bu ünitenin tüm meydan okumalarını içerecek klasörde yeni bir `.env` dosyasına kopyalayın:

    ```bash
    cp .env.gemini.sample ../.env
    ```

3. `.env` dosyasını favori kod editörünüzle açın:

    ```bash
    code ../.env
    ```

4. `.env` dosyasında, `your_gemini_api_key` kısmını aldığınız API anahtarıyla değiştirin.

5. Dosyayı kaydedin ve kapatın.

</details>


## ✅ Kurulumunuzu kontrol edin

```bash
jupyter notebook check.ipynb
```

## 🏁 Tamamlandı

Artık LLM'lerle çalışmak için taze bir ortamınız var.

Her zaman `langchain-env` ortamını kullandığınızı kontrol etmeyi unutmayın. Özellikle VS Code kullanırken, bu yeni ortamı seçtiğinizden emin olun.

Bu projeyi commit etmeyi ve push yapmayı unutmayın.

---

## 🛠️ My Setup Notes

### Folder structure

The env is pinned to a wrapper folder for the whole unit, not to `~/projects`:

```
~/projects/
├── llm-unit/                      # pyenv local → langchain-env
│   ├── .python-version
│   ├── .env                       # shared credentials, outside any repo
│   └── S19D3-S-data-langchain-env/
└── ...                            # other projects → workintech env
```

`pyenv local` applies to the folder it's set in and all its subfolders. Pinning it to `llm-unit/` means every challenge in this unit uses `langchain-env` automatically, while the rest of `~/projects` keeps the bootcamp env. Keeping `.env` in `llm-unit/` also guarantees it can't be committed.

### Creating the environment

```bash
cd ~/projects/llm-unit
pyenv virtualenv 3.12.9 langchain-env
pyenv local langchain-env
pip install --upgrade pip
cd S19D3-S-data-langchain-env
pip install -r requirements.txt
```

### Running both environments in parallel

The active env is resolved by directory, so there is no manual activate/deactivate:

```bash
cd ~/projects/llm-unit/S19D3-S-data-langchain-env
pyenv version                  # langchain-env
pip list | grep -i langchain   # langchain packages present

cd ~/projects
pyenv version                  # workintech
pip list | grep -i langchain   # empty → no leakage
```

### Authentication: Gemini API key instead of Vertex AI

The GCP free trial on my account had ended (both billing accounts closed), so Vertex AI couldn't be enabled. I used the standalone Gemini Developer API instead:

```bash
cp .env.gemini.sample ../.env   # then set GEMINI_API_KEY
```

`load_dotenv()` searches parent folders, so it finds `llm-unit/.env`, and `genai.Client()` reads `GEMINI_API_KEY` automatically. Switching back to Vertex only requires replacing `../.env` with the Vertex template.

### Model change in `check.ipynb`

`gemini-2.5-flash-lite` returns `404 NOT_FOUND` for new users. I replaced it with `gemini-3.5-flash-lite`, which the API error message recommends. Note that `client.models.list()` still lists 2.5, but calling it fails.

### Using the env in VS Code

When opening a notebook: **Select Kernel → Python Environments → `langchain-env`** (`~/.pyenv/versions/langchain-env/bin/python`). Running `jupyter` from a terminal inside `llm-unit/` picks it up automatically.
