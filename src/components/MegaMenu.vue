<template>
  <div class="l-mega-menu" style="top: 115px;">
    <div class="l-mega-menu-inner">
      <i class="c-megamenu-close icon-symbol-close"></i>
      <div class="l-mega-menu-column"
        v-for="column in list.Columns"
        :key="column.Title">
        <h2 class="c-mega-menu-column-headline">{{ column.Title }}</h2>
        <div class="c-mega-menu-list"
            v-for="section in column.Sections"
            :key="section.Section">
          <h3 class="c-mega-menu-headline">{{ section.Name }}</h3>
          <ul>
            <li v-for="child in section.Children"
            :key="child.HeaderPage">
              <a 
                class="c-mega-menu-link" 
                :href="child.HeaderPage">{{ child.Title }}</a>
            </li>
            </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MegaMenu",
  props: ["list"]
};
</script>
