<template>
    <Page class="page">
        <ActionBar class="action-bar">
            <!-- 
            Use the NavigationButton as a side-drawer button in Android
            because ActionItems are shown on the right side of the ActionBar
            -->
            <NavigationButton ios:visibility="collapsed" icon="res://menu" @tap="onDrawerButtonTap"></NavigationButton>
            <!-- 
            Use the ActionItem for IOS with position set to left. Using the
            NavigationButton as a side-drawer button in iOS is not possible,
            because its function is to always navigate back in the application.
            -->
            <ActionItem icon="res://menu" android:visibility="collapsed" @tap="onDrawerButtonTap" ios.position="left"></ActionItem>
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>

        <GridLayout class="page__content">
            <Label class="page__content-icon fas" text.decode="&#xf015;"></Label>
            <Label class="page__content-placeholder" :text="message"></Label>
        </GridLayout>

        <ListView for="item in items" @itemTap="onItemTap">
            <v-template>
                <!-- Shows the list item label in the default color and style. -->
                <Label :text="item" />
            </v-template>
        </ListView>

    </Page>
</template>

<script>
    import * as utils from "~/shared/utils";
    import SelectedPageService from "../shared/selected-page-service";
    import Smoothie from "./Smoothie";

    export default {
        components: {
            Smoothie
        },
        data() {
            return {
                items: [
                    'Something Green',
                    'PB & Berry',
                    'Power Me',
                    'Fresh Mint',
                    'Melissa',
                ],
            }
        },
        mounted() {
            SelectedPageService.getInstance().updateSelectedPage("Home");
        },
        computed: {
            message() {
                return "This is a smoothie app";
            }
        },
        methods: {
            onDrawerButtonTap() {
                utils.showDrawer();
            },
            onItemTap(event) {
                console.log(event.item);
                this.$navigateTo(Smoothie, {
                    transition: {
                        name:'fade',
                        duration: 200
                    },
                    props: {
                        smoothieName: event.item, 
                    }
                });
                console.log(event.item);
            }
        }
    };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '~@nativescript/theme/scss/variables/blue';
    // End custom common variables
    .page__content-placeholder {
        color: black;
    }

    .list-item {
        color: red;
    }

    // Custom styles
</style>
