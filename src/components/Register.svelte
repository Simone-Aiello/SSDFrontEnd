<script lang="ts">
    import axios from 'axios';
    import { push } from 'svelte-spa-router';
    import Navbar from './Navbar.svelte';
    let current_form_data = {
        username: null,
        email: null,
        password1: null,
        password2: null,
    }
    let form_error = {
        username: null,
        email: null,
        password1: null,
        password2: null,
        alert: null,
    }
    const getCookieValue = (name: string) => (
      document.cookie.match('(^|;)\\s*' + name + '\\s*=\\s*([^;]+)')?.pop() || ''
    )

    async function onRegister() {
        console.log(current_form_data)
        let error = false
        for (let key in form_error){
            form_error[key] = null;
        }
        for (let key in current_form_data){
            if(current_form_data[key] == null || current_form_data[key] == ""){
                form_error[key] = "This field may not be blank"
                error = true
            }
        }
        if(current_form_data.email != null && current_form_data.email.match(/^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/) == null){
            form_error.email = "Enter a valid email address"
            error = true
        }
        if (error) return;
        axios.post('http://localhost:8000/api/v1/auth/registration/',{
            username: current_form_data.username,
            email: current_form_data.email,
            password1: current_form_data.password1,
            password2: current_form_data.password2,
            headers: {
                    'x-csrftoken': getCookieValue('csrftoken'),
                },
        }).then(() =>{
            push("/login")
        }).catch((error) =>{
            console.log(error.response.data)
            if(error.response.data.non_field_errors != undefined){
                form_error.alert = error.response.data.non_field_errors[0]
            }
            else{
                for(let key in error.response.data){
                    form_error[key] = error.response.data[key]
                }
            }
        })
    }
</script>

<main>
    <Navbar />
    <div id="title">
        <h1>Create an account</h1>
    </div>
    <div class="container">
        <div class="row">
            <div class="col-2"></div>
            <div class="col-8">
                {#if form_error.alert != null}
                    <div class="alert alert-danger" role="alert">
                        {form_error.alert}
                    </div>
                {/if}
                <form>
                    <!-- Username input -->
                    <div class="form-outline mb-4">
                        {#if form_error.username != null}
                            <input type="text" id="form2Example1" class="form-control is-invalid" bind:value={current_form_data.username}/>
                            <div class="invalid-feedback">{form_error.username}</div>
                            <label class="form-label" for="form2Example1">Username</label>
                        {:else}
                            <input type="text" id="form2Example1" class="form-control" bind:value={current_form_data.username}/>
                            <label class="form-label" for="form2Example1">Username</label>
                        {/if}
                    </div>
                
                    <!-- Email input -->
                    <div class="form-outline mb-4">
                        {#if form_error.email != null}
                            <input type="text" id="form2Example1" class="form-control is-invalid" bind:value={current_form_data.email}/>
                            <div class="invalid-feedback">{form_error.email}</div>
                            <label class="form-label" for="form2Example1">Email</label>
                        {:else}
                            <input type="text" id="form2Example1" class="form-control" bind:value={current_form_data.email}/>
                            <label class="form-label" for="form2Example1">Email</label>
                        {/if}
                    </div>

                    <!-- Password1 input -->
                    <div class="form-outline mb-4">
                        {#if form_error.password1 != null}
                            <input type="password" id="form2Example2" class="form-control is-invalid" bind:value={current_form_data.password1}/>
                            <div class="invalid-feedback">{form_error.password1}</div>
                            <label class="form-label" for="form2Example2">Password</label>
                        {:else}
                            <input type="password" id="form2Example2" class="form-control" bind:value={current_form_data.password1}/>
                            <label class="form-label" for="form2Example2">Password</label>
                        {/if}
                    </div>
                
                    <!-- Confirm password1 -->
                        <div class="form-outline mb-4">
                            {#if form_error.password2 != null}
                                <input type="password" id="form2Example2" class="form-control is-invalid" bind:value={current_form_data.password2}/>
                                <div class="invalid-feedback">{form_error.password2}</div>
                                <label class="form-label" for="form2Example2">Confirm Password</label>
                            {:else}
                                <input type="password" id="form2Example2" class="form-control" bind:value={current_form_data.password2}/>
                                <label class="form-label" for="form2Example2">Confirm Password</label>
                            {/if}
                        </div>
                    <!-- 2 column grid layout for inline styling -->
                    <div class="row mb-4">
                    <div class="col d-flex justify-content-center">

                    </div>
                
                    </div>
                
                    <!-- Submit button -->
                    <button id="submit-button" type="button" class="btn btn-primary btn-block mb-4" on:click={onRegister}>Register</button>
                </form>
            </div>
            <div class="col-2"></div>
        </div>
    </div>
</main>

<style>
#title{
    margin-top: 12px;
    margin-bottom: 20px;
    display: flex;
    justify-content: center;
}
.container{
    margin: auto;
    width: 60%;
}
#submit-button{
    width: 100%;
}
</style>