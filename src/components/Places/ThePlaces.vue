<template>
  <main>
    <section class="places">
      <div class="wrap">
        <h2 class="block-title">Заповедные места русской истории</h2>
        <span class="places__txt"
          >Мы продумали досуг жильцов, чтобы свободные вечера проходили с
          комфортом (здесь нужно описать задумку ландшафтного дизайнера с
          бульваром и преимущество парковой зоны внутри ЖК + наличие
          коммерческой недвижимости)</span
        >
        <ul :style="galleryStyle" class="places__gallery">
          <li
            v-for="item in items"
            v-on:click="increase"
            :class="item.class"
            :key="item.key"
          >
            <ThePlace :imgSrc="item.src" :altTxt="item.alt" />
          </li>
        </ul>
        <div class="arrow-btns">
          <button class="arrow-btns__btn" v-on:click="swipe"></button>
          <button
            class="arrow-btns__btn arrow-btns__btn--right"
            v-on:click="swipe"
          ></button>
        </div>
      </div>
    </section>
    <div v-on:click="hide" class="blure"></div>
  </main>
</template>

<script>
export default {
  name: "ThePlaces",

  components: {
    ThePlace: () => import("../Place/ThePlace.vue"),
  },

  data() {
    return {
      items: [],
      galleryPos: 0,
      step: 0,
      units: "",
      maxScroll: 0,
    };
  },

  computed: {
    galleryStyle: function () {
      return { left: this.galleryPos + this.units };
    },
  },

  methods: {
    swipe(evt) {
      if (evt.target.classList.contains("arrow-btns__btn--right")) {
        if (this.galleryPos === 0)
          this.toggleBtn(
            document.querySelector(".arrow-btns__btn--off"),
            false
          );
        if (this.galleryPos !== -this.step * this.maxScroll)
          this.galleryPos -= this.step;
        if (this.galleryPos === -this.step * this.maxScroll)
          this.toggleBtn(evt.target, true);
      } else {
        if (this.galleryPos === -this.step * this.maxScroll)
          this.toggleBtn(
            document.querySelector(".arrow-btns__btn--right"),
            false
          );
        if (this.galleryPos !== 0) this.galleryPos += this.step;
        if (this.galleryPos === 0) this.toggleBtn(evt.target, true);
      }
    },

    toggleBtn(btn, toggle) {
      if (toggle) {
        btn.setAttribute("style", "animation-name: none;");
        btn.classList.add("arrow-btns__btn--off");
      } else {
        btn.removeAttribute("style");
        btn.classList.remove("arrow-btns__btn--off");
      }
    },

    ifResize() {
      if (
        window.innerWidth > 675 &&
        window.innerWidth <= 900 &&
        this.maxScroll !== 5
      ) {
        this.galleryPos = 0;
        this.step = 318;
        this.units = "px";
        this.maxScroll = 5;

        this.toggleBtn(document.querySelector(".arrow-btns__btn"), true);
      } else if (window.innerWidth > 900 && this.maxScroll !== 2) {
        this.galleryPos = -51;
        this.step = 51;
        this.units = "%";
        this.maxScroll = 2;

        this.toggleBtn(document.querySelector(".arrow-btns__btn"), false);
      } else if (window.innerWidth <= 675 && this.maxScroll !== 6) {
        this.maxScroll = 6;
      }
    },

    increase(evt) {
      const windowWidth = window.innerWidth;
      const width = windowWidth * 0.8;

      const imgAlt = evt.target.getAttribute("alt");
      const imgNum = imgAlt.substring(imgAlt.length - 1, imgAlt.length);

      const img = document.createElement("img");
      const wrap = document.createElement("div");
      const btnClose = document.createElement("button");

      const blure = document.querySelector(".blure");

      blure.setAttribute("style", "display: flex");

      wrap.setAttribute("style", "display: flex; position: relative;");

      img.setAttribute("alt", imgAlt);
      img.setAttribute(
        "src",
        require("../../img/pic-place-" + imgNum + ".jpg")
      );
      img.setAttribute(
        "style",
        `width: ${width}px; max-width: 1300px; border-radius: 3%`
      );

      btnClose.setAttribute("class", "btn-close");
      btnClose.addEventListener("click", this.hide);

      wrap.appendChild(img);
      wrap.appendChild(btnClose);
      blure.appendChild(wrap);
    },

    hide() {
      const wrap = document.querySelector(".blure");
      wrap.replaceChildren();
      wrap.setAttribute("style", "display: none");
    },
  },

  mounted() {
    const names = [
      "place-1",
      "place-2",
      "place-3",
      "place-4",
      "place-5",
      "place-6",
      "place-7",
    ];

    names.forEach((el) => {
      this.items.push({
        class: "places__li " + el,
        key: el,
        src: require("../../img/pic-" + el + ".jpg"),
        alt: "Заповедное место " + el.substring(el.length - 1, el.length),
      });
    });

    const windowWidth = window.innerWidth;

    if (windowWidth <= 900) {
      this.galleryPos = 0;
      this.step = 318;
      this.units = "px";
      this.maxScroll = windowWidth <= 675 ? 6 : 5;

      this.toggleBtn(document.querySelector(".arrow-btns__btn"), true);
    } else {
      this.galleryPos = -51;
      this.step = 51;
      this.units = "%";
      this.maxScroll = 2;
    }

    window.addEventListener("resize", this.ifResize);
  },
};
</script>
