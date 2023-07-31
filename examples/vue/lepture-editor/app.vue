<template>

    <div v-if="show" class="modal fixed w-full h-full top-0 left-0 flex items-center justify-center">
        <div @click="$emit('close')" class="absolute w-full h-full bg-gray-900 opacity-50"></div>
        <div class="z-10 bg-white shadow-md p-10 animate__animated animate__fadeInDown">
            <form method="post" action="/api/workspace">

                <!-- Hidden input -->
                <input type="hidden" name="issue_id" :value="issue.id" />

                <p class="text-xl font-semibold">
                    Edit markdown
                </p>

                <div class="my-3">

                    <textarea id="modal-edit-description" :value="issue.description" name="description" placeholder="Enter new description.." class="border w-full px-3 py-2"></textarea>
                </div>

                <p class="mt-4 text-center">

                    <button class="bg-green-500 text-white px-3 py-2 rounded font-semibold hover:bg-green-400 mr-3">
                        Update
                    </button>
                    <button @click="$emit('close')" class="hover:font-semibold">
                        Cancel
                    </button>
                </p>
            </form>
        </div>
    </div>

</template>

<script>
    module.exports = {
        props: {
            show: Boolean,
            issue: Object
        },
        watch: {

            show(val) {

                if (val) {

                    /* 1. Mount editor when DOM is loaded */
                    Vue.nextTick(() => {

                        this.mountEditor()
                    })
                }
            }
        },
        data() {
            return {
                /* Editor support */
                editor: null
            }
        },
        methods: {

            mountEditor() {

                /* Does not work if using beforeDestroy as v-if is used */
                this.editor = null

                if (this.editor == null) {

                    this.editor = new Editor({
                        element: document.getElementById('modal-edit-description')
                    })
                    this.editor.render()
                    /* Codemirror bindings */
                    this.editor.codemirror.on("change", (cm, change) => {
                        this.issue.description = cm.getValue()
                    })

                }
            }
        }
    }

</script>
