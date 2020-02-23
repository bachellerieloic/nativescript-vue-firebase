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
            <ListView for="(item, key) in getItems" @itemTap="onItemTap">
                <v-template>
                    <Label :text="item.price"/>
                </v-template>
            </ListView>
        </GridLayout>
    </Page>
</template>

<script>
    import * as utils from "~/shared/utils";
    import SelectedPageService from "../shared/selected-page-service";
    import Smoothie from "./Smoothie";
    const firebase = require("nativescript-plugin-firebase");

    export default {
        components: {Smoothie},
        data() {
            return {
                items: {},
            }
        },
        mounted() {
            SelectedPageService.getInstance().updateSelectedPage("Home");
            // listen to changes in the /smoothie path
            firebase.addChildEventListener(this.onChildEvent, "/smoothies").then(
                function(listenerWrapper) {
                    var path = listenerWrapper.path;
                    var listeners = listenerWrapper.listeners; // an Array of listeners added
                    // you can store the wrapper somewhere to later call 'removeEventListeners'
                }
            )
        },
        computed: {
            message() {
                return "This is a smoothie app";
            },
            async getItems() {
                let $this = this;
                console.log('********** GET SMOOTHIES **********');
                firebase.getValue('/smoothies')
                    .then(result => {
                        Object.keys(result.value).forEach(function(key) {
                            $this.items[key] = result.value[key];
                            console.log(key + ' - ' + $this.items[key].price);
                        });
                        return $this.items;
                    }).catch(error => console.log("Error: " + error));
            }
        },
        methods: {
            onChildEvent (result) {
                this.items[result.key] = JSON.stringify(result.value);
                console.log("Event type: " + result.type);
                console.log("Key: " + result.key);
                console.log("Value: " + JSON.stringify(result.value));
            },
            onDrawerButtonTap() {
                utils.showDrawer();
            },
            onItemTap(event) {
                console.log(event.item);
                console.log('pushing');
                // firebase.push('smoothies/', { 'name' : event.item }
                // ).then(
                //     function (result) {
                //         console.log("created key: " + result.key);
                //     }
                // );
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
