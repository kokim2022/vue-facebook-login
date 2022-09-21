<template>
    <div>
        <h2>You're logged in with Vue.js & Facebook!!</h2>
        <p>All accounts from secure api end point:</p>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>Id</th>
                    <th>Facebook Id</th>
                    <th>Name</th>
                    <th>Extra Info</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="account in accounts" :key="account.id">
                    <td>{{account.id}}</td>
                    <td>{{account.facebookId}}</td>
                    <td>{{account.name}}</td>
                    <td>{{account.extraInfo}}</td>
                    <td>
                        <button>
                            Connect With Facebook Pages
                        </button>
                    </td>
                </tr>
                <tr v-if="!accounts">
                    <td colspan="5" class="text-center">
                        <span class="spinner-border spinner-border-lg align-center"></span>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import { accountService } from '@/_services';

export default {
    data() {
        return {
            accounts: null
        }
    },
    created() {
        accountService.getAll()
            .then(x => this.accounts = x);
    },
    methods: {
        deleteAccount(id) {
            // show deleting spinner
            this.accounts = this.accounts.map(x => {
                if (x.id === id) x.isDeleting = true;
                return x;
            });

            // delete account
            accountService.delete(id)
                .then(() => this.accounts = this.accounts.filter(x => x.id !== id));
        }
    }
}
</script>