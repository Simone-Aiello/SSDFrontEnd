<script lang="ts">
    import axios from 'axios';
    import Navbar from './Navbar.svelte';
    import {push} from 'svelte-spa-router'
    let current_form_data = {
        "username": "",
        "password": ""
    }
    let alert_displayed = null;
    let password_data = {
        valid: null,
        message: "",
    };
    let username_data = {
        valid: null,
        message: "",
    }
    async function doLogin() {
        alert_displayed = null;
        password_data.valid = null;
        username_data.valid = null;

        if (current_form_data.username == ""){
            username_data.valid = false;
            username_data.message = "This field is required";
        }
        else if(current_form_data.password == ""){
            password_data.valid = false;
            password_data.message = "This field is required";
            return;
        }
        else{
            axios.post('http://localhost:8000/api/v1/auth/login/',{
                username: current_form_data.username,
                password: current_form_data.password
            }, {
                withCredentials: true
            }).then(response =>{
                push('/')
            }).catch(error =>{
                console.log(error)
                alert_displayed = error.response.data.non_field_errors[0]
            })
        }
    }
</script>

<main>
    <Navbar />
    <div class="container">
        <div class="row">
            <div class="col-2"></div>
            <div class="col-8">
                {#if alert_displayed != null}
                    <div class="alert alert-danger" role="alert">
                        {alert_displayed}
                    </div>
                {/if}
                <form>
                    <!-- Username input -->
                    <div class="form-outline mb-4">
                    {#if username_data.valid != null && !username_data.valid}
                        <input type="text" id="form2Example1" class="form-control is-invalid" bind:value={current_form_data.username}/>
                        <div class="invalid-feedback">{username_data.message}</div>
                        <label class="form-label" for="form2Example1">Username</label>
                    {:else}
                        <input type="text" id="form2Example1" class="form-control" bind:value={current_form_data.username}/>
                        <label class="form-label" for="form2Example1">Username</label>
                    {/if}
                    </div>
                
                    <!-- Password input -->
                    <div class="form-outline mb-4">
                    {#if password_data.valid != null && !password_data.valid}
                        <input type="password" id="form2Example2" class="form-control is-invalid" bind:value={current_form_data.password}/>
                        <div class="invalid-feedback">{password_data.message}</div>
                        <label class="form-label" for="form2Example2">Password</label>
                    {:else}
                        <input type="password" id="form2Example2" class="form-control" bind:value={current_form_data.password}/>
                        <label class="form-label" for="form2Example2">Password</label>
                    {/if}

                </div>
                
                    <!-- 2 column grid layout for inline styling -->
                    <div class="row mb-4">
                    <div class="col d-flex justify-content-center">

                    </div>
                
                    </div>
                
                    <!-- Submit button -->
                    <button type="button" class="btn btn-primary btn-block mb-4" on:click={doLogin}>Sign in</button>
                
                    <!-- Register buttons -->
                    <div class="text-center">
                    <p>Not a member? <a href="#!">Register</a></p>
                    </div>
                </form>
            </div>
            <div class="col-2"></div>
        </div>
    </div>
</main>

<style>

</style>