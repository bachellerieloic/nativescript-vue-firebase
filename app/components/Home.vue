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

<!--        <ListView for="item in items" @itemTap="onItemTap">s-->
<!--            <v-template>-->
<!--                &lt;!&ndash; Shows the list item label in the default color and style. &ndash;&gt;-->
<!--                <Label :text="item" />-->
<!--            </v-template>-->
<!--        </ListView>-->
    </Page>
</template>

<script>
    import * as utils from "~/shared/utils";
    import SelectedPageService from "../shared/selected-page-service";
    import Smoothie from "./Smoothie";
    const firebase = require("nativescript-plugin-firebase");

    export default {
        components: {
            Smoothie
        },
        data() {
            return {
                items: {
                    'name': 'test'
                },
            }
        },
        mounted() {
            SelectedPageService.getInstance().updateSelectedPage("Home");
            let $this = this;
            console.log('********** GET VALUES **********');
            firebase.getValue('/smoothies')
                .then(result => {
                        console.log(JSON.stringify(result));
                        $this.items = JSON.stringify(result);
                        Object.keys(result).map(function(key, index) {
                            $this.items.name = result['value']['name']);
                        });
                    }
                )
                .catch(error => console.log("Error: " + error));

            console.log($this.items);
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
                console.log('pushing');
                firebase.push('smoothies/', { 'name' : event.item }
                ).then(
                    function (result) {
                        console.log("created key: " + result.key);
                    }
                );
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
