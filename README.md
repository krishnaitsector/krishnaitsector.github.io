<div class="images">
  <img onclick="openModal(0)" id="image0" />
  <img onclick="openModal(1)" id="image1" />
  <img onclick="openModal(2)" id="image2" />
  <img onclick="openModal(3)" id="image3" />
</div>

<div id="lightbox" class="lightbox">
  <button onclick="closeModal()" class="close-btn">
    Close
  </button>

  <div class="image-preview">
    <img id="preview-image" />
  </div>

  <div class="control-btns">
    <button onclick="control(-1)" class="control-left">
      &lt;
    </button>
    <button onclick="control(1)" class="control-left">
      &gt;
    </button>
  </div>

  <div id="modal-images-block" class="lightbox__images">
    <img onclick="openModal(0)" id="l-image0" />
    <img onclick="openModal(1)" id="l-image1" />
    <img onclick="openModal(2)" id="l-image2" />
    <img onclick="openModal(3)" id="l-image3" />
  </div>
</div>

The CSS
* {
  box-sizing: border-box;
}

.images {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 20px;
}

.images img {
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.lightbox {
  position: absolute;
  left: 0;
  top: 0;
  padding: 0 50px 30px;
  width: 100%;
  height: 100vh;
  background-color: rgb(18, 7, 7);
  display: none;
  flex-direction: column;
}

.lightbox.visible {
  display: flex;
}

.lightbox .close-btn {
  width: 80px;
  align-self: flex-end;
  height: 40px;
  margin: 20px 0;
}

.lightbox .image-preview {
  width: 100%;
  margin: 0 auto;
  flex: 1;
  height: 100%;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.image-preview img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.control-btns {
  position: relative;
  top: -10px;
  margin: 0 auto;
}

.control-btns button {
  cursor: pointer;
}

.control-left {
  margin-right: 50px;
}

.lightbox__images {
  height: 300px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 20px;
  align-items: center;
}

.lightbox__images img {
  width: 100%;
  opacity: 0.3;
  cursor: pointer;
}

.lightbox__images img.active {
  width: 100%;
  opacity: 1;
}

The JavaScript
const IMAGE0 =
  "https://i.picsum.photos/id/229/400/200.jpg?hmac=ULnwo8IFtjR3PshWPNEvFWNU8Xwl_OIeUtVmZIQanhU"
const IMAGE1 =
  "https://i.picsum.photos/id/154/400/200.jpg?hmac=uhKcJIPoFcq2xMC16yvZAwA8sTeIbThUr-Njq0DkhSU"
const IMAGE2 =
  "https://i.picsum.photos/id/690/400/200.jpg?hmac=kOkDXkZEUaSUQviVm67apRu5EPMD_L0rHfKVt32iogQ"
const IMAGE3 =
  "https://i.picsum.photos/id/633/400/200.jpg?hmac=-axbA3Zg3r_xPYOy7OdaIb5yTFDBKubd9LYJrnwpHeU"

const images = [IMAGE0, IMAGE1, IMAGE2, IMAGE3]

const image0 = document.getElementById("image0")
const image1 = document.getElementById("image1")
const image2 = document.getElementById("image2")
const image3 = document.getElementById("image3")

const lightbox = document.getElementById("lightbox")

const previewImg = document.getElementById("preview-image")

const modalImagesBlock = document.getElementById(
  "modal-images-block"
)

image0.src = IMAGE0
image1.src = IMAGE1
image2.src = IMAGE2
image3.src = IMAGE3

let activeId = null
previewImg.src = images[0]

const modalImagesElements =
  modalImagesBlock.getElementsByTagName("img")

const modalImages = Object.values(modalImagesElements)

modalImages.forEach((imageElement, i) => {
  console.log(imageElement)
  imageElement.src = images[i]
})

function openModal(imgId) {
  if (activeId !== null) {
    modalImages[activeId].classList.remove("active")
  }

  activeId = imgId

  lightbox.classList.add("visible")

  previewImg.src = images[imgId]

  modalImages[imgId].classList.add("active")
}

function closeModal() {
  lightbox.classList.remove("visible")
}

function control(direction) {
  const prevId = activeId
  if (direction === 1) {
    // next
    activeId =
      activeId + 1 > images.length - 1
        ? // then go to the beginning
          (activeId = 0)
        : (activeId = activeId + 1)
  } else {
    // previous
    activeId =
      activeId - 1 < 0
        ? // then go to the end
          (activeId = images.length - 1)
        : activeId - 1
  }

  previewImg.src = images[activeId]
  modalImages[activeId].classList.add("active")
  modalImages[prevId].classList.remove("active")
}

