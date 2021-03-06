<template lang="pug">
.speaker__container
  modal(v-if="modalVisible", @close="modalVisible = false")
    div(slot="body")
      .speaker__large
        img.speaker__image(:src="image", :alt="speaker.name")
        .speaker__aside
          .speaker__title
            | {{ speaker.name }}
          .speaker__subtitle
            | {{ translate(speaker.title) }}
          .speaker__social
            a.icon.icon--github(v-if="speaker.github", :href="`https://github.com/${speaker.github}`" target="_blank")
            a.icon.icon--gitlab(v-if="speaker.gitlab", :href="`https://gitlab.com/${speaker.gitlab}`" target="_blank")
            a.icon.icon--twitter(v-if="speaker.twitter", :href="`https://twitter.com/${speaker.twitter}`" target="_blank")
            a.icon.icon--medium(v-if="speaker.medium", :href="`https://medium.com/${speaker.medium}`" target="_blank")
        .speaker__bio
          | {{ translate(speaker.bio) }}
        blockquote.speaker__quote(v-if="speaker.quote")
          | {{ translate(speaker.quote) }}
          cite {{ speaker.name }}
  .speaker(@click="showDetails")
    img.speaker__image(:src="image", :alt="speaker.name")
    .speaker__aside
      span.speaker__label(v-if="speaker.label")
        | {{ translate(speaker.label) }}
      .speaker__title
        | {{ speaker.name }}
      .speaker__subtitle
        | {{ translate(speaker.title) }}
      .speaker__social
        a.icon.icon--gitlab(@click.stop="", v-if="speaker.gitlab", :href="`https://gitlab.com/${speaker.gitlab}`" target="_blank")
        a.icon.icon--github(@click.stop="", v-if="speaker.github", :href="`https://github.com/${speaker.github}`" target="_blank")
        a.icon.icon--twitter(@click.stop="", v-if="speaker.twitter", :href="`https://twitter.com/${speaker.twitter}`" target="_blank")
        a.icon.icon--medium(@click.stop="", v-if="speaker.medium", :href="`https://medium.com/${speaker.medium}`" target="_blank")

      button.button-secondary(@click.stop="showDetails") {{ $t('speakers.more') }}
</template>

<script>
import Modal from './Modal'

export default {
  components: { Modal },
  props: {
    speaker: {
      type: Object,
      required: true
    },
    adjustImgUrl: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      modalVisible: false,
      lang: 0
    }
  },
  mounted () {
    this.lang = this.$i18n.locale == 'en' ? 1 : 0
  },
  methods: {
    showDetails () {
      this.modalVisible = true
      ga('send', 'event', {
        eventAction: 'click',
        eventCategory: `Speaker details ${this.speaker.name}`
      })
    },
    translate (data) {
      return Array.isArray(data) ? data[this.lang] : data 
    }
  },
  computed: {
    image () {
      return this.adjustImgUrl
        ? `/../${this.speaker.img}`
        : this.speaker.img
    }
  }
}
</script>

<style lang="sass">
@import ~assets/css/base/helpers
@import ~assets/css/base/variables
@import ~assets/css/components/card

.speaker__container
  width: calc(100% - 20px)
  display: inline-block
  margin: 10px
  height: 100px
  position: relative
  text-align: left

  @media #{$medium-up}
    width: calc(50% - 20px)
    height: 145px

.speaker
  position: absolute
  width: 100%
  @include card

.speaker__image
  max-width: 100px
  display: inline-block
  padding: 0
  vertical-align: top
  margin-right: 20px
  border-radius: 5px
  border-top-right-radius: 0
  border-bottom-right-radius: 0

  @media #{$medium-up}
    max-width: 140px

.speaker__aside
  display: inline-block
  vertical-align: top
  max-width: calc(100% - 120px)

  @media #{$medium-up}
    max-width: calc(100% - 160px)

  .speaker__label
    position: absolute
    background-color: $color-vue-green
    padding: 5px
    right: -20px
    border-radius: 6px
    font-size: 13px
    color: $color-white
    transform: rotate(25deg)

.speaker__title
  margin-top: 12px
  font-size: 14px
  letter-spacing: 0.8px
  font-weight: 600
  color: $color-text
  text-transform: uppercase

  @media #{$medium-up}
    font-size: 18px

.speaker__subtitle
  font-size: 14px
  font-weight: 300
  color: #8795a9

  @media #{$medium-up}
    font-size: 16px

.speaker__large
  .speaker__aside
    max-width: calc(100% - 160px)

    @media #{$medium-up}
      max-width: calc(100% - 260px)

  .speaker__image
    max-width: 140px
    border-top-right-radius: 5px
    border-bottom-right-radius: 5px

    @media #{$medium-up}
      max-width: 240px

  .speaker__title
    margin-top: -5px
    font-size: 28px
    text-transform: none

  .speaker__subtitle
    font-size: 18px

.speaker__bio,
.speaker__quote
  margin-top: 20px
  font-size: 16px
  font-weight: 300
  color: #495669

  @media #{$medium-up}
    font-size: 18px

.speaker__quote
  margin: 20px 0 0 0
  font-style: italic

  cite
    display: block
    margin-top: 10px

    &:before
      content: "— "

.speaker__social
  margin-top: 4px

  @media #{$medium-up}
    margin-top: 10px

  .icon--twitter
    margin-left: 15px

.speaker
  .button-secondary
    position: absolute
    right: 10px
    bottom: 10px

  .button-secondary,
    display: none

    @media #{$medium-up}
      display: inline-block

.button-secondary
  background: none
  border: none
  text-transform: uppercase
  padding: 10px
  line-height: 15px
  font-size: 15px
  font-weight: 400
  color: $color-link
  letter-spacing: 0.8px

  &:hover
    background: #f8f8f8
</style>
