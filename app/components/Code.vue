<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!" />
        <StackLayout>
            <Button @tap="onTap" class="btn btn-primary" :text="Tap the button"></Button>

            <ListView for="item in itemsObs">
                <v-template>
                    <Label :text="item" />
                </v-template>
            </ListView>
        </StackLayout>
    </Page>
</template>

<script lang="ts">
    import { ObservableArray } from "tns-core-modules/data/observable-array";

    export default {
        data() {
            return {
                counter: 3,
                // items: ["item1", "item2"],
                itemsObs: new ObservableArray(["item1", "item2"])
            };
        },

        methods: {
            onTap() {
                const newItem = "item" + this.counter++;
                // this.items.push(newItem);
                this.itemsObs.push(newItem);
            }
        }
    };
</script>

<style scoped>
    ...
</style>
