<template lang="pug">
.about(:class="{'is-active': $store.state.about}")
  .about__texture
  .about__image
    ul.image__list
      li.list__item.ball_1
        img.list__ball(src="/assets/img/about_img_ball_01.png", alt="ABOUT COURT SIDE" width="792" height="785")
      li.list__item.ball_2
        img.list__ball(src="/assets/img/about_img_ball_02.png", alt="ABOUT COURT SIDE" width="792" height="785")
      li.list__item.ball_3
        img.list__ball(src="/assets/img/about_img_ball_03.png", alt="ABOUT COURT SIDE" width="792" height="785")
      li.list__item.ball_4
        img.list__ball(src="/assets/img/about_img_ball_04.png", alt="ABOUT COURT SIDE" width="792" height="785")
      li.list__item.ball_5
        img.list__ball(src="/assets/img/about_img_ball_05.png", alt="ABOUT COURT SIDE" width="792" height="785")
      li.list__item.ball_6
        img.list__ball(src="/assets/img/about_img_ball_06.png", alt="ABOUT COURT SIDE" width="792" height="785")
    img.image__heading(ref="aboutHeading" src="/assets/img/about_img_heading.svg", alt="" width="168" height="74")
    picture
      source(media="(max-width: 750px)", srcset="/assets/img/about_img_txt_sp.svg")
      img.image__text(ref="aboutText" src="/assets/img/about_img_txt.svg", alt="")
  .about__close(@click="About()")
    | CLOSE
</template>

<script>
import gsap from 'gsap'

const easeOutBack = (x) => {
  const c1 = 5;
  const c3 = c1 + 1;

  return 1 + c3 * Math.pow(x - 1, 3) + c1 * Math.pow(x - 1, 2);
}

export default {
  data(){
    return{
      aboutBallCurrent: 0,
      aboutBallCurrentMax: 6,
      animeTime: 2,
      animCount: 7,
      time: 0,
      timeMax: 0,
      stepTime: 0,
      stepMax: 0,
      step: 0,
      scale: 0,
      alpha: 0,
    }
  },
  methods:{
    LoopStart(count, sec){
      this.time = 0
      this.timeMax = sec * 1000
      this.stepTime = this.timeMax / count
      this.stepMax = count

      setTimeout(this.LoopAnime, this.stepTime)

      const animeDelay = this.animeTime + 0.4
      this.aboutHeadingAnime = gsap.to(this.$refs.aboutHeading,{
          delay: animeDelay,
          duration: 1,
          alpha: 1,
        })
      this.aboutTextAnime = gsap.to(this.$refs.aboutText,{
        delay: animeDelay,
        duration: 1,
        alpha: 1,
      })
    },
    LoopAnime(){
      const easeValue = easeOutBack(this.time / this.timeMax)

      this.step = Math.round(easeValue * this.stepMax)
      this.scale = easeValue
      this.aboutBallCurrent = Math.round(this.step) % this.aboutBallCurrentMax

      for (let index = 0; index < this.aboutBallCurrentMax; index++) {
        this.alpha = (index == this.aboutBallCurrent) ? 1.0 : 0.0
        gsap.set('.ball_'+(index + 1),{
          alpha: this.alpha,
          scale: this.scale
        })
      }

      this.time += this.stepTime

      if(this.time < this.timeMax){
        this.LoopTimeOut = setTimeout(this.LoopAnime, this.stepTime)
      }
      // else{
      //   this.LoopStart(this.animCount * this.aboutBallCurrentMax + 1, this.animeTime)
      // }
    },
    About(){
      if(this.$store.state.about){
        this.LoopStart(this.animCount * this.aboutBallCurrentMax + 1, this.animeTime)
      } else{
        this.aboutHeadingAnime.paused(true);
        this.aboutTextAnime.paused(true);

        gsap.set(this.$refs.aboutHeading,{
          duration: 0.7,
          alpha: 0,
        })
        gsap.set(this.$refs.aboutText,{
          duration: 0.7,
          alpha: 0,
        })
        for (let index = 0; index < this.aboutBallCurrentMax; index++) {
          gsap.set('.ball_'+(index + 1),{
            duration: 0.7,
            alpha: 0
          })
        }

        clearTimeout(this.LoopTimeOut)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.about{
  @include transition(700);
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #000;
  z-index: 3;
  pointer-events: none;
  opacity: 0;
  .about__image{
    display: flex;
    align-items: center;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: auto;
    width: 61.875vw;
    max-width: 792px;
    will-change: opacity, transform;
    @include mq('sp'){
      top: 50%;
      margin: 0 -9.6560846561vw 0 -6.656085vw;
      width: calc(100vw + 19.312169312169312vw);
      max-width: none;
      transform: translateY(-50%);
    }
    .image__list{
      width: 100%;
    }
    .list__item{
      transform: scale(0);
    }
    .list__ball{
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      margin: auto;
      width: 100%;
      height: auto;
    }
    .image__heading{
      position: absolute;
      top: -38vw;
      right: 0;
      bottom: 0;
      left: 0;
      margin: auto;
      width: 21.158690176322418%;
      opacity: 0;
      @media screen and (min-width: 1281px) {
        top: -486px;
      }
      @include mq('sp'){
        top: -74vw;
        right: 3%;
        width: 30%;
      }
    }
    .image__text{
      position: absolute;
      top: -4vw;
      right: -21%;
      bottom: 0;
      left: 0;
      margin: auto;
      width: 62.247474747474747%;
      opacity: 0;
      @media screen and (min-width: 1281px) {
        top: -52px;
      }
      @include mq('sp'){
        top: 12vw;
        right: -8%;
        width: 70%;
      }
    }
  }

  .about__close{
    @include font('bebas-neue-pro');
    position: absolute;
    top: 0;
    bottom: 0;
    left: 34px;
    margin: auto 0;
    height: 1em;
    color: #DBD6D0;
    font-size: 1.5rem;
    letter-spacing: 0.03em;
    cursor: pointer;
    @include mq('sp'){
      top: 64px;
      left: 24px;
      bottom: auto;
      font-size: 1.3rem;
    }
    &::after{
      content: "";
      display: inline-block;
      position: relative;
      bottom: 2px;
      margin-left: 8px;
      width: 18px;
      height: 18px;
      background-image: url(/assets/img/about_close.svg);
      vertical-align: middle;
    }
  }

  .about__footer{
    position: absolute;
    right: 34px;
    bottom: 20px;
    @include mq('sp'){
      display: none;
    }
    .footer__logo{
      display: block;
      margin: 0 0 28px auto;
      width: 79px;
    }
    .footer__cs{
      display: block;
      margin: 0 0 6px auto;
    }
    .footer__share{
      display: block;
      margin: 0 0 4px auto;
    }
    .footer__sns{
      display: flex;
      justify-content: space-between;
      margin: 0 0 12px auto;
      width: 79px;
    }
  }
}
.about__texture{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url(/assets/img/about_texture.png);
  background-repeat: repeat;
  background-size: cover;
  pointer-events: none;
}
.about.is-active{
  pointer-events: auto;
  opacity: 1;
}
</style>