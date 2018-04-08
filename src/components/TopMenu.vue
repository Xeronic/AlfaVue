<template>
<div class="c-top-menu">
    <a 
        class="c-top-menu-link" 
        v-for="sectionLink in sectionLinks"
        :key="sectionLink.Href" 
        :href="sectionLink.Href" 
        :title="sectionLink.Text"
        v-text="sectionLink.Text"
    />

    <a 
        class="c-top-menu-link c-top-menu-link-light" 
        v-for="specialLink in specialLinks"
        :key="specialLink.Href" 
        :href="specialLink.Href" 
        :title="specialLink.Text"
        v-text="specialLink.Text"
    />
    
    <a 
        class="c-top-menu-link c-top-menu-link-fat" 
        v-for="boldSpecialLink in boldSpecialLinks"
        :key="boldSpecialLink.Href" 
        :href="boldSpecialLink.Href" 
        :title="boldSpecialLink.Text"
        v-text="boldSpecialLink.Text"
        target="_blank"
    />
</div>
</template>

<script>
export default {
  name: "TopMenu",
  data() {
    return {
      sectionLinks: [],
      specialLinks: [],
      boldSpecialLinks: []
    };
  },
  created() {
    fetch(
      "https://test.alfalaval.com/webapi/TopNavigation/GetTopNavigation/?language=en&pageId=9081"
    )
      .then(result => result.json())
      .then(json => {
        this.sectionLinks = json.SectionLinks;
        this.specialLinks = json.SpecialLinks;
        this.boldSpecialLinks = json.BoldSpecialLinks;
      });
  }
};
</script>