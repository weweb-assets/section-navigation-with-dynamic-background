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
    data() {
        return {
            backgroundColor: this.content.backgroundColor,
            boxShadow: 'none',
            resizeObserver: null,
            resizeListenerHolder: null,
            scrollListenerHolder: null,
        };
    },
    computed: {
        cssVariables() {
            return {
                '--backgroundColor': this.backgroundColor,
                '--boxShadow': this.boxShadow,
            };
        },
    },
    mounted() {
        this.init();
    },
    beforeUnmount() {
        this.removeListeners();
    },
    methods: {
        init() {
            this.refreshObservers();

            if (!this.scrollListenerHolder)
                this.scrollListenerHolder = document.addEventListener('scroll', this.refreshObservers);
        },
        refreshObservers() {
            this.scrollListener();
        },
        scrollListener() {
            if (window.scrollY > 0) {
                this.backgroundColor = this.content.backgroundColorWhenUserStartsScrolling;
                this.boxShadow = '0px 1px 10px rgba(78, 50, 75, 0.2)';
            } else {
                this.backgroundColor = this.content.backgroundColor;
                this.boxShadow = 'none';
            }
        },
        removeListeners() {
            if (this.scrollListenerHolder)
                this.scrollListenerHolder = document.removeEventListener('scroll', this.refreshObservers);
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
    box-shadow: var(--boxShadow);
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
