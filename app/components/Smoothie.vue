<template>
    <Page class="page">
        <ActionBar :title="smoothieName" class="action-bar" />

            <!-- Use the NavigationButton as a side-drawer button in Android
            because ActionItems are shown on the right side of the ActionBar-->
            <!-- <NavigationButton ios:visibility="collapsed" icon="res://menu" @tap="onDrawerButtonTap"></NavigationButton> -->
            <NavigationButton text="Back" @tap="navigateBack"/>
            <!-- Use the ActionItem for IOS with position set to left. Using the
            NavigationButton as a side-drawer button in iOS is not possible,
            because its function is to always navigate back in the application.-->
            <!-- <ActionItem icon="res://menu" android:visibility="collapsed" @tap="onDrawerButtonTap" ios.position="left"></ActionItem> -->

        <GridLayout class="page__content">
            <Label class="page__content-icon far" text.decode="&#xf1ea;"></Label>
            <Label class="page__content-placeholder" :text="smoothieName"></Label>
            <Button text="Back" @tap="$navigateBack" />
        </GridLayout>

    </Page>
</template>

<script>
    import * as utils from "~/shared/utils";
    import SelectedPageService from "../shared/selected-page-service";

    export default {
        props: ['smoothieName'],
        data() {
            return {};
        },
        mounted() {
            SelectedPageService.getInstance().updateSelectedPage("Browse");
        },
        computed: {
            message() {
                return "<!-- Page content goes here -->";
            }
        },
        methods: {
            navigateBack() {
                this.$navigateBack();
            },
            onDrawerButtonTap() {
                utils.showDrawer();
            }
        }
    };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '~@nativescript/theme/scss/variables/blue';
    // End custom common variables

    // Custom styles
</style>
