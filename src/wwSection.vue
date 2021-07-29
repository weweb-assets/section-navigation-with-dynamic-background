<template>
    <div class="navigation-menu" :style="cssVariables">
        <div class="navigation-default">
            <wwLayout class="navigation-default__layout" path="navbarContent"></wwLayout>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        content: { type: Object, required: true },
    },
    wwDefaultContent: {
        navbarContent: [],
    },
    data() {
        return {
            backgroundColor: this.content.backgroundColor,
            resizeObserver: null,
            resizeListenerHolder: null,
            scrollListenerHolder: null,
        };
    },
    computed: {
        cssVariables() {
            return {
                '--backgroundColor': this.backgroundColor,
            };
        },
    },
    mounted() {
        this.init();

        this.resizeObserver = new ResizeObserver(() => {
            this.getNavbarData();
        });

        this.resizeObserver.observe(this.$el);
    },
    beforeUnmount() {
        this.resizeObserver.unobserve(this.$el);
        this.removeListeners();
    },
    methods: {
        init() {
            this.refreshObservers();

            if (!this.scrollListenerHolder)
                this.scrollListenerHolder = document.addEventListener('scroll', this.refreshObservers);
            if (!this.resizeListenerHolder)
                this.resizeListenerHolder = document.addEventListener('resize', this.refreshObservers);
        },
        refreshObservers() {
            this.scrollListener();
        },
        scrollListener() {
            if (window.scrollY > 0) {
                this.backgroundColor = this.content.backgroundColorWhenUserStartsScrolling;
            } else {
                this.backgroundColor = this.content.backgroundColor;
            }
        },
        removeListeners() {
            if (this.scrollListenerHolder)
                this.scrollListenerHolder = document.removeEventListener('scroll', this.refreshObservers);
            if (this.resizeListenerHolder)
                this.resizeListenerHolder = document.removeEventListener('resize', this.refreshObservers);
        },
    },
};
</script>

<style lang="scss" scoped>
.navigation-menu {
    --backgroundColor: rgb(250, 250, 250);

    position: fixed;
    z-index: 9;
    display: flex;
    background-color: var(--backgroundColor);
    transition: all 300ms;

    .navigation-default {
        position: relative;
        width: 100%;
        &__layout {
            display: flex;
            flex-direction: column;
            min-width: 100px;
        }
    }
}
</style>
