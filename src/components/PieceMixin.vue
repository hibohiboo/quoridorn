<script>
import { mapState, mapActions, mapGetters } from 'vuex'
import AddressCalcMixin from './AddressCalcMixin'

export default {
  mixins: [AddressCalcMixin],
  props: {
    'type': { type: String, required: true },
    'objKey': { type: String, required: true }
  },
  data () {
    return {
      isHover: false
    }
  },
  mounted () {},
  methods: {
    ...mapActions([
      'windowOpen',
      'setProperty'
    ]),
    leftDown () {
      if (this.storeObj.isLock || this.isRolling) {
        this.$emit('leftDown')
        return
      }
      // console.qLog(`  [methods] mousedown left on ${this.type}`)
      const offset = {
        w: this.mouseOnTable.x - this.rect.left,
        h: this.mouseOnTable.y - this.rect.top
      }
      const pieceObj = {
        move: {
          from: {
            x: this.mouseOnTable.x,
            y: this.mouseOnTable.y
          },
          gridOffset: {
            x: Math.floor(offset.w / this.gridSize),
            y: Math.floor(offset.h / this.gridSize)
          }
        },
        isDraggingLeft: true
      }
      this.setProperty({property: `public.${this.type}.list.${this.storeIndex}`, value: pieceObj, logOff: true})
    },
    leftUp () {
      if (this.storeObj.isLock || this.isRolling) {
        this.$emit('leftUp')
        return
      }
      // console.qLog(`  [methods] mouseup left on ${this.type}`)
      const locate = {
        x: this.mouseOnTable.x - this.storeObj.move.gridOffset.x * this.gridSize,
        y: this.mouseOnTable.y - this.storeObj.move.gridOffset.y * this.gridSize
      }
      if (this.isFitGrid) {
        locate.x = (Math.ceil(locate.x / this.gridSize) - 1) * this.gridSize
        locate.y = (Math.ceil(locate.y / this.gridSize) - 1) * this.gridSize
      }
      const pieceObj = {
        left: locate.x,
        top: locate.y,
        move: {
          dragging: {
            x: 0,
            y: 0
          },
          gridOffset: {
            x: 0,
            y: 0
          }
        },
        isDraggingLeft: false
      }
      this.setProperty({property: `public.${this.type}.list.${this.storeIndex}`, value: pieceObj, logOff: true, isNotice: true})
    },
    rightDown () { if (this.storeObj.isLock || this.isRolling) { this.$emit('rightDown') } },
    rightUp (event) {
      this.setProperty({property: `map.isOverEvent`, value: true})
      this.$emit('rightUp', event)
    },
    openContext (event, contextProperty) {
      let pageX = event.pageX
      let pageY = event.pageY

      const obj = {
        key: this.objKey,
        x: pageX,
        y: pageY
      }
      this.setProperty({property: contextProperty, value: obj, logOff: true})
      this.windowOpen(contextProperty)
      console.qLog(`  [methods] open context => ${contextProperty}(${this.objKey})`)
    },
    mouseover () {
      this.isHover = true
    },
    mouseout () {
      this.isHover = false
    },
    arrangeAngle (angle) {
      if (angle > 180) { angle -= 360 }
      if (angle < -180) { angle += 360 }
      return angle
    },
    getAngle (mouseOnTable) {
      const rect = this.rect
      const center = {
        x: rect.left + rect.width / 2,
        y: rect.top + rect.height / 2
      }
      // 中心座標を基準としたマウス座標
      const loc = {
        x: (mouseOnTable.x - center.x),
        y: (mouseOnTable.y - center.y)
      }
      // 中心点と指定された座標とを結ぶ線の角度を求める
      const angle = Math.atan2(loc.y, loc.x) * 180 / Math.PI
      return angle
    },
    rollStart () {
      this.setProperty({property: `map.rollObj.isRolling`, value: true, logOff: true})
      console.qLog(`  [methods] rolling start on ${this.type}(${this.objKey})`)
      const angle = this.getAngle(this.mouseOnTable)
      const planeAngle = this.arrangeAngle(angle - this.angle.total)
      // console.qLog(`angle:${angle}, total:${this.angle.total}, dragStartB:${this.angle.dragStart}, dragStartA:${planeAngle}`)
      this.setProperty({property: `public.${this.type}.list.${this.storeIndex}.angle.dragStart`, value: planeAngle, logOff: true})
      const obj = {
        propName: this.type,
        key: this.objKey
      }
      this.setProperty({property: `map.rollObj`, value: obj, logOff: true})
    },
    rollEnd (event) {
      // console.qLog(`rollEnd`, event.pageX, event.pageY)
      const mapObj = {
        rollObj: {
          isRolling: false
        }
      }
      if (event.button === 2) {
        mapObj.isOverEvent = true
      }
      this.setProperty({property: `map`, value: mapObj, logOff: true})
      const planeAngle = this.arrangeAngle(this.angle.dragging + this.angle.total)
      const total = this.arrangeAngle(Math.round(planeAngle / 30) * 30)
      // console.qLog(`angle:${angle}, planeAngle:${planeAngle}, totalB:${this.angle.total}, totalA:${total}`)
      const obj = {
        total: total,
        dragging: 0
      }
      this.setProperty({property: `public.${this.type}.list.${this.storeIndex}.angle`, value: obj, isNotice: true, logOff: true})
    }
  },
  watch: {
    mouseOnTable: {
      handler (mouseOnTable) {
        // console.qLog(`piece:${this.storeObj.name}, isDraggingLeft:${this.storeObj.isDraggingLeft}, isRolling:${this.isRolling}`)
        if (this.isRolling) {
          if (!this.isThisRolling) {
            return
          }
          const angle = this.getAngle(mouseOnTable)
          const dragging = this.arrangeAngle(this.arrangeAngle(angle - this.angle.dragStart) - this.angle.total)
          this.setProperty({property: `public.${this.type}.list.${this.storeIndex}.angle.dragging`, value: dragging, logOff: true})
        } else {
          if (this.storeObj.isDraggingLeft) {
            const obj = {
              x: mouseOnTable.x - this.storeObj.move.from.x,
              y: mouseOnTable.y - this.storeObj.move.from.y
            }
            this.setProperty({property: `public.${this.type}.list.${this.storeIndex}.move.dragging`, value: obj, logOff: true})
          }
        }
      },
      deep: true
    }
  },
  computed: mapState({
    ...mapGetters([
      'isFitGrid',
      'getPieceObj'
    ]),
    storeObj () { return this.getPieceObj(this.type, this.objKey) },
    storeIndex (state) { return state.public[this.type].list.indexOf(this.storeObj) },
    rollObj: state => state.map.rollObj,
    angle () { return this.storeObj.angle },
    rect () {
      return {
        top: this.storeObj.top + this.storeObj.move.dragging.y,
        left: this.storeObj.left + this.storeObj.move.dragging.x,
        width: this.storeObj.columns * this.gridSize,
        height: this.storeObj.rows * this.gridSize
      }
    },
    isRolling: state => state.map.rollObj.isRolling,
    isThisRolling () { return this.$store.state.map.rollObj.isRolling && this.rollObj.propName === this.type && this.rollObj.key === this.objKey },
    isFix () { return this.storeObj.isFix },
    currentAngle () { return this.arrangeAngle(this.angle.total + this.angle.dragging) },
    style () {
      const rectObj = this.rect
      return {
        top: `${rectObj.top}px`,
        left: `${rectObj.left}px`,
        width: `${rectObj.width}px`,
        height: `${rectObj.height}px`,
        transform: `rotateZ(${this.arrangeAngle(Math.round(this.currentAngle / 30) * 30)}deg)`
      }
    },
    marginGridSize () {
      return this.$store.state.public.map.margin.gridSize
    },
    gridSize () {
      return this.$store.state.public.map.grid.size
    }
  })
}
</script>
