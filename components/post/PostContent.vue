<script>
import Vue from 'vue'
import VueWithCompiler from 'vue/dist/vue.esm'
import ComplexImage from '../cms/ComplexImage'
import IYDIframe from '../cms/IYDIframe'
import Carousel from '../cms/Carousel'

Vue.component('complex-image', ComplexImage)
Vue.component('iyd-iframe', IYDIframe)
Vue.component('carousel', Carousel)

export default Vue.component('post-content', {
  components: { ComplexImage },
  props: {
    content: {
      type: String,
      default() {}
    }
  },
  render(h) {
    const div = document.createElement('div')
    const content = this.content.replace(/<a/g, '<a target="__blank"')
    div.innerHTML = content
    div.classList.add('blog-post__content')
    return h(VueWithCompiler.compile(div.outerHTML))
  }
})
</script>

<style lang="scss">
.blog-post__content {
  padding-bottom: 10rem;
  h2,
  h3,
  h4 {
    color: $grey-dark;
    font-weight: $font-weight--light;
    padding: rem(56px) 0 rem(30px);
  }
  p,
  ul li,
  ol li {
    font-family: 'Lora', serif;
    line-height: 1.83;
    color: #747474;
    a {
      color: $link-blue;
      font-weight: $font-weight--bold;
      word-break: break-word;
    }
  }
  p {
    padding-bottom: rem(40px);
  }
  ul,
  ol {
    padding-left: 1.5rem;
    padding-bottom: 20px;
    li {
      padding-bottom: rem(20px);
    }
  }
  img,
  video,
  iframe.external-video {
    width: 70%;
    margin: 0 15% rem(40px);
  }
  iframe {
    &.external-video {
      height: 30vw;
    }
  }
  tr td {
    background: #eee;
    padding: 0.5rem 1rem;
  }
  tr th {
    background: #ddd;
    padding: 0.5rem 1rem;
  }
  blockquote {
    text-align: center;
    font-style: italic;
    line-height: 1.83;
    color: #747474;
    padding-bottom: 2.5rem;
  }
  @media screen and (max-width: $breakpoint__tablet--max) {
    img,
    video,
    iframe.external-video {
      width: 100%;
      margin: rem(40px) 0;
    }
  }
  @media screen and (max-width: $breakpoint__mobile--max) {
    iframe.external-video {
      height: 50vw;
    }
    tr {
      display: flex;
      flex-direction: column;
    }
  }
}
$mask-background-color: rgba(0, 0, 0, 0.7);
.lightbox {
  &__container {
    position: fixed;
    width: 100vw;
    height: 100vh;
    left: 0;
    top: 0;
    z-index: 3;
    animation: maskIn $transitions-duration forwards;

    &--animation-close {
      animation: maskOut $transitions-duration forwards;
    }
  }

  &__content {
    cursor: pointer;
    width: 100%;
    height: 100%;
    margin: 0 auto;
    padding: 4rem 6rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__media {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    transition-duration: 0ms;
    transition: transform $transitions-duration--short ease-out, width ease-out;

    &--initial-position {
      transform: scale(1) translate3d(0, 0, 0);
    }
  }

  &__close-btn {
    position: fixed;
    top: rem(14px);
    right: rem(20px);
    font-size: $font-size--small;
    color: black;
    cursor: pointer;
  }

  @keyframes maskIn {
    0% {
      background: transparent;
    }
    100% {
      background: $mask-background-color;
    }
  }

  @keyframes maskOut {
    0% {
      background: $mask-background-color;
    }
    100% {
      background: transparent;
    }
  }
  @media only screen and (max-width: $breakpoint__mobile--max) {
    &__media--images {
      max-width: rem(350px);
      padding: rem(25px);
    }
  }
}
</style>
