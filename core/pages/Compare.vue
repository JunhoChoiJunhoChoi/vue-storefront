<template>
  <div class="compare">
    Core Compare page
    <!-- Items in compare -->
    <ul>
      <li v-for="(product, index) in items" :key="index">
        {{ product.name | htmlDecode }}
      </li>
    </ul>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Composite from 'core/mixins/composite'
import i18n from 'core/lib/i18n'

export default {
  name: 'Compare',
  metaInfo () {
    return {
      title: this.$route.meta.title || this.$props.title || i18n.t('Compare Products'),
      meta: this.$route.meta.description ? [{vmid: 'description', description: this.$route.meta.description}] : []
    }
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  mixins: [Composite],
  created () {
    this.$store.dispatch('compare/load')
    this.$store.dispatch('attribute/list', {
      filterValues: [true],
      filterField: 'is_user_defined'
    })
  },
  methods: {
    removeFromCompare (product) {
      this.$store.dispatch('compare/removeItem', product)
    }
  },
  computed: {
    ...mapGetters({
      attributesByCode: 'attribute/attributeListByCode',
      attributesById: 'attribute/attributeListById'
    }),
    items () {
      return this.$store.state.compare.itemsCompare
    },
    all_comparable_attributes () {
      return Object.values(this.attributesByCode).filter(a => {
        return parseInt(a.is_comparable)
      })
    }
  }
}
</script>
