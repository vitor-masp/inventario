<template>
    <div
        class="modal fade"
        id="createCategoryModal"
        tabindex="-1"
        role="dialog"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
    >
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">
                        Adicionar Categoria
                    </h5>
                    <button
                        type="button"
                        class="close"
                        data-dismiss="modal"
                        aria-label="Close"
                        id="close-add-category"
                    >
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <form
                        class="container-fluid"
                        @keydown="form.errors.clear($event.target.name)"
                        @submit.prevent="send"
                    >
                        <div class="row">
                            <div class="col-md-12">
                                <div class="form-group bmd-form-group">
                                    <label class="bmd-label-floating"
                                        >Nome</label
                                    >
                                    <input
                                        type="text"
                                        class="form-control"
                                        name="category-name"
                                        id="category-name"
                                        v-model="form.name"
                                        required
                                        autofocus
                                    />
                                    <small
                                        class="text-danger"
                                        v-text="form.errors.get('name')"
                                        v-if="form.errors.has('name')"
                                    ></small>
                                </div>
                            </div>
                            <submit-button />
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Form from "../../form-validation/Form";
export default {
    data() {
        return {
            form: new Form({
                name: "",
            }),
        };
    },
    methods: {
        send: _.throttle(
            function() {
                window.events.$emit("loading", true);
                this.form
                    .post("/categories")
                    .then((result) => {
                        window.flash("Categoria cadastrada com sucesso!");
                        window.events.$emit("loading", false);
                        window.events.$emit("new_category", result);
                        document.querySelector("#close-add-category").click();
                    })
                    .catch((errors) => {
                        window.flash("Algo deu errado.", "danger");
                        window.events.$emit("loading", false);
                    });
            },
            1000,
            { trailing: false }
        ),
    },
};
</script>
