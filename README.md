# ClipReplay - YouTube Video Bölümlerini Döngüye Alma Uzantısı

**ClipReplay**, YouTube videolarındaki belirli bölümleri sürekli olarak izlemek isteyen kullanıcılar için geliştirilmiş bir Chrome uzantısıdır. Bu uzantı, kullanıcıların video içerikleri üzerinde daha fazla kontrol sahibi olmasını sağlar. Özellikle eğitim amaçlı videolar veya müzik pratikleri gibi durumlar için son derece faydalıdır. Bu uzantı sayesinde, videoların yalnızca belirli bir kısmını seçip döngüye alarak, sürekli tekrar izleme işlemini zahmetsizce yapabilirsiniz.


<div align=center >
   <img src="https://github.com/user-attachments/assets/6b5746e2-3323-46bf-938b-7711cbe9508d" alt="Logo" width=50%>
</div>

## Özellikler
- **Video Döngüsü:** YouTube videolarında belirlediğiniz zaman aralığını döngüye alır, kesintisiz bir izleme deneyimi sunar.
- **Zaman Aralığı Seçimi:** Başlangıç ve bitiş zamanlarını kolayca belirleyerek sadece istediğiniz kısmı izleyebilirsiniz.
- **Kolay Kullanım:** Basit ve anlaşılır kullanıcı arayüzü ile video bölümleri arasındaki geçişleri hızlıca ayarlayabilirsiniz.
- **Farklı Kullanım Senaryoları:** Eğitim videoları, müzik pratiği, belirli bir içeriğin tekrar tekrar izlenmesi gibi bir çok farklı amaçla kullanılabilir.

## Kurulum
1. **Chrome Uzantısını Yükleyin:**
   - Bu uzantıyı yüklemek için, `manifest.json` dosyasının bulunduğu klasöre gidin.
   - Chrome'da `chrome://extensions/` sayfasına gidin.
   - Sağ üst köşedeki **Geliştirici modu**nu etkinleştirin.
   - **Paketlenmemiş uzantıyı yükle** butonuna tıklayın ve indirdiğiniz klasörü seçin.

2. **Uzantıyı Aktif Hale Getirin:**
   - Uzantı, simgesine tıkladığınızda aktif olur ve YouTube videolarında çalışmaya başlar.

## Kullanım
Uzantıyı yükledikten sonra, aşağıdaki adımları takip ederek bir video aralığını döngüye alabilirsiniz:

1. YouTube üzerinde bir video açın.
2. Uzantının simgesine tıklayın.
3. Başlangıç ve bitiş zamanlarını (dakika:saniye) girin.
4. **Set Loop** butonuna tıklayın.
5. Video, belirlediğiniz zaman aralığında kesintisiz bir şekilde döngüye girecektir.

## Ana Sayfa
<div align=center >
   <img src="https://github.com/user-attachments/assets/35beed87-89c4-406d-be17-9e45525c94fa" alt="Ana Sayfa" width=50%>
</div>
Uzantı, kullanıcıların video döngüsü ayarlarını yapabilmesi için basit ve kullanıcı dostu bir popup arayüzüne sahiptir. Başlangıç ve bitiş zamanlarını girip, video döngüsünü başlatmak için aşağıdaki ekran görüntüsünü kullanabilirsiniz:
Bu arayüz, kullanıcıların videolarını döngüye almak için gerekli başlangıç ve bitiş zamanlarını kolayca girmelerini sağlar. "Loop Ayarla" butonuna tıkladığınızda video belirtilen zaman aralığında döngüye girer.

## Loop Başlatıldığında
<div align=center >
   <img src="https://github.com/user-attachments/assets/3a8806a0-b70b-405d-84e0-efd364bf3157" alt="Loop Başlatıldığında" width=50%>
</div>
Döngü başlatıldığında, popup penceresinde ayarlanan zaman dilimiyle birlikte "Loop Başlatıldı" mesajı gösterilir ve döngü işlemi başlar. Aşağıda bu ekranın bir örneğini bulabilirsiniz:


## Loop'u Durdurmak
Video döngüsünü durdurmak için, popup penceresinde yer alan "Loop'u Durdur" butonuna tıklayarak işlemi sonlandırabilirsiniz. Bu işlem, video zamanlayıcısını sıfırlayacak ve döngüyü durduracaktır. Aşağıda durdurma ekranının görselini görebilirsiniz:

## Kod Yapısı

### `background.js`

Bu dosya, uzantının video oynatma ve durdurma fonksiyonlarını içerir. Ayrıca, uzantının simgesine tıklandığında videoyu oynatıp duraklatmak için kullanılan işlevi barındırır.

```javascript
chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    function: togglePlayPause
  });
});

function togglePlayPause() {
  const video = document.querySelector('video');
  if (video) {
    if (video.paused) {
      video.play();
    } else {
      video.pause();
    }
  } else {
    alert("Bu sayfada video yok.");
  }
}
```

### `manifest.json`

Bu dosya, uzantının temel yapılandırmasını ve gerekli izinleri içerir. YouTube'a özgü host izinleri ve uzantının ikonu gibi bilgiler burada tanımlanır.

```json
{
  "manifest_version": 3,
  "name": "ClipReplay",
  "version": "1.0",
  "description": "YouTube videolarının belirli bölümlerini kesip tekrar izleyebilmeniz için kolay bir araç.",
  "permissions": [
    "scripting",
    "activeTab"
  ],
  "host_permissions": [
    "https://www.youtube.com/*"
  ],
  "icons": {
    "16": "icons/ClipReplayLogo.png",
    "48": "icons/ClipReplayLogo.png",
    "128": "icons/ClipReplayLogo.png"
  },
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icons/ClipReplayLogo.png",
      "48": "icons/ClipReplayLogo.png",
      "128": "icons/ClipReplayLogo.png"
    }
  },
  "background": {
    "service_worker": "background.js"
  }
}
```

### `popup.css`

Uzantının popup penceresinin stilini tanımlar. Video döngüsü ayarlama arayüzü bu stil dosyasına dayanır.

```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background: #202020;
  color: #fff;
  width: 300px;
  height: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  text-align: center;
  width: 90%;
}

h1 {
  font-size: 20px;
  margin-bottom: 15px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

label {
  font-size: 14px;
  text-align: left;
}

input {
  padding: 8px;
  font-size: 14px;
  border-radius: 5px;
  border: none;
  margin-top: 7px;
  margin-bottom: 7px;
}

button {
  background: #b60808;
  color: #fff;
  border: none;
  padding: 10px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background: #cc0000;
}

.status {
  margin-top: 10px;
  font-size: 14px;
  color: #ccc;
}

#stopButton {
  display: none;
}

#loopForm {
  display: block;
}

#stopButton.show {
  display: block;
}

#loopForm.hide {
  display: none;
}
```

### `popup.html`

Popup arayüzünün HTML yapısını içerir. Kullanıcılar, video döngüsünü burada ayarlayabilirler.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ClipReplay</title>
  <link rel="stylesheet" href="popup.css">
</head>
<body>
  <div class="container">
    <h1>ClipReplay</h1>
    <form id="loopForm">
      <div>
        <label for="start">Başlangıç (dakika:saniye)</label>
        <input type="text" id="start" placeholder="dakika:saniye -> örnek: 1:20" required>
      </div>
      <div>
        <label for="end">Bitiş (dakika:saniye)</label>
        <input type="text" id="end" placeholder="dakika:saniye -> örnek: 3:45" required>
      </div>
      <button type="submit">Loop Ayarla</button>
    </form>
    <p id="statusText" class="status">Videonuz için loop ayarlayın!</p>
    <button id="stopButton" class="stopButton" style="display:none;">Loop'u Durdur</button>
  </div>
  <script src="popup.js"></script>
</body>
</html>
```

### `popup.js`

Bu dosya, popup arayüzünde kullanıcının girdiği veriyi işleyip döngüye alma işlemini başlatan JavaScript kodlarını içerir. Ayrıca, kullanıcı döngüyü durdurduğunda yerel depolama verilerini temizler.

```javascript
document.addEventListener('DOMContentLoaded', () => {

  // burada yerel hafızadan getirdim, eğer tıklanmışsa stop butonu gözükecek
  const isLooping = localStorage.getItem('isLooping') === 'true';
  const startTime = localStorage.getItem('startTime');
  const endTime = localStorage.getItem('endTime');
  
  // burada hangi itemler gözükecek karar veriyor
  if (isLooping) {
    document.getElementById('stopButton').style.display = "inline-block";
    document.getElementById('loopForm').style.display = "none";
    document.getElementById('statusText').textContent = `Loop ayarlandı: ${startTime} - ${endTime}`;
  } else {
    document.getElementById('stopButton').style.display = "none";
    document.getElementById('loopForm').style.display = "block";
  }
});

// tıklandığında başlayacak
document.getElementById('loopForm').addEventListener('submit', (event) => {

  event.preventDefault(); // bu sayfa yüklemesin diye yazdım
  
  // hepsini const olarak tanımladım, temiz bir js kodu yazmak için
  const startInput = document.getElementById('start').value;
  const endInput = document.getElementById('end').value;

  const startSeconds = timeToSeconds(startInput);
  const endSeconds = timeToSeconds(endInput);


  chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    if (tabs.length > 0) {
      chrome.scripting.executeScript({
        target: { tabId: tabs[0].id },
        func: loopVideo,
        args: [startSeconds, endSeconds],
      });
    }
  });

  // burada yerele kaydediyorum
  localStorage.setItem('isLooping', 'true');
  localStorage.setItem('startTime', startInput);
  localStorage.setItem('endTime', endInput);

  // bilgileri kullanıcıya yazdırıyorum
  document.getElementById('statusText').textContent = `Loop ayarlandı: ${startInput} - ${endInput}`;
  document.getElementById('stopButton').style.display = "inline-block";
  document.getElementById('loopForm').style.display = "none";
});

// durdur tıklandığında
document.getElementById('stopButton').addEventListener('click', () => {
  chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    if (tabs.length > 0) {
      chrome.scripting.executeScript({
        target: { tabId: tabs[0].id },
        func: stopLoop,
      });
    }
  });

  localStorage.removeItem('isLooping');
  localStorage.removeItem('startTime');
  localStorage.removeItem('endTime');

  document.getElementById('statusText').textContent = "Loop durduruldu!";
  document.getElementById('stopButton').style.display = "none";
  document.getElementById('loopForm').style.display = "block";
});

// her seferinde tekrar çalışacak
function loopVideo(start, end) {
  const video = document.querySelector('video');
  if (video) {
    video.currentTime = start;

    video.addEventListener('timeupdate', () => {
      if (video.currentTime >= end) {
        video.currentTime = start;
      }
    });
  } else {
    alert("Bu sayfada video yok.");
  }
}

// durdur değinde sayfa geri yüklensin, böylece tüm işlemler sıfırlanır
function stopLoop() {
  const video = document.querySelector('video');
  if (video) {
    window.location.reload();
  }
}

// zamanı hesaplamak için ayrı bir fonkisyon yazdım
function timeToSeconds(time) {
  const parts = time.split(':');
  const minutes = parseInt(parts[0]);
  const seconds = parseInt(parts[1]);
  return (minutes * 60) + seconds;
}

```



