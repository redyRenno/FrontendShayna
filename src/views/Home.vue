<template>
  <div class="home">
    <HeaderShayna />
    <HeroShayna />
    <WomanShayna />
    <InstaShayna />
    <PartnerShayna />
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import HeroShayna from '@/components/HeroShayna';InstaShayna
import WomanShayna from '@/components/WomanShayna';
import InstaShayna from '@/components/InstaShayna';
import PartnerShayna from '@/components/PartnerShayna';
import FooterShayna from '@/components/FooterShayna';

export default {
  name: 'Home',
  components: {
    HeaderShayna,
    HeroShayna,
    WomanShayna,
    InstaShayna,
    PartnerShayna,
    FooterShayna
  }
}
</script>
