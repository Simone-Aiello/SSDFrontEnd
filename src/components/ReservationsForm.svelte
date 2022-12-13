<script lang="ts">
    import axios from "axios";
    import { push } from "svelte-spa-router";
    import Navbar from "./Navbar.svelte";
    const getCookieValue = (name: string) => (
      document.cookie.match('(^|;)\\s*' + name + '\\s*=\\s*([^;]+)')?.pop() || ''
    )
    let free_umbrella = []
    let form_error = {
        number_of_seats: null,
        reservation_start_date: null,
        reservation_end_date: null,
        reserved_umbrella_id: null,
        alert: null,
    }
    let new_reservation_data = {
        number_of_seats: null,
        reservation_start_date: "",
        reservation_end_date: "",
        reserved_umbrella_id: null,
    }
    async function dateChanged(){
        if (new_reservation_data.reservation_end_date != "" && new_reservation_data.reservation_start_date != ""){
            await axios.get(`http://localhost:8000/api/v1/beachreservation/freeumbrella?start_date=${new_reservation_data.reservation_start_date}&end_date=${new_reservation_data.reservation_end_date}`,
            {
                withCredentials: true,
            })
            .then((response) =>{
                free_umbrella = response.data
            })
            .catch((error) =>{
                form_error.reservation_end_date = error.response.data[0]
                free_umbrella = []
            })
        }
    }
    async function postNewReservation(){
        let error = false
        for (let key in form_error){
            form_error[key] = null;
        }
        for (let key in new_reservation_data){
            if(new_reservation_data[key] == null || new_reservation_data[key] == ""){
                form_error[key] = "This field is required"
                error = true
            }
        }

        if (new_reservation_data.reservation_start_date > new_reservation_data.reservation_end_date){
            form_error.reservation_end_date = "End date can't be before start date"
            error = true
        }

        if (error) return;
        axios.post('http://localhost:8000/api/v1/beachreservation/',{
            number_of_seats: new_reservation_data.number_of_seats,
            reservation_start_date: new_reservation_data.reservation_start_date,
            reservation_end_date: new_reservation_data.reservation_end_date,
            reserved_umbrella_id: new_reservation_data.reserved_umbrella_id,
        },{
            headers: {
                'x-csrftoken': getCookieValue('csrftoken'),
            },
            withCredentials: true
        }).then((data) =>{
            push('/reservations')
        }).catch((error) =>{
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
    <h1 id="title"> Make a new reservation</h1>
    <div class="container">
        <div class="row">
            <div class="col-2"></div>
            <div class="col-8">
                {#if form_error.alert != null}
                    <div class="alert alert-danger" role="alert">
                    </div>                    
                {/if}
                    <form>
                        <!-- Number of seats input -->
                        <div class="form-outline mb-4">
                            {#if form_error.number_of_seats != null}
                                <input type="number" id="form2Example1" class="form-control is-invalid" bind:value={new_reservation_data.number_of_seats}/>
                                <div class="invalid-feedback">{form_error.number_of_seats}</div>
                            {:else}
                                <input type="number" id="form2Example1" class="form-control" bind:value={new_reservation_data.number_of_seats}/>
                            {/if}
                            <label class="form-label" for="form2Example1">Number of seats</label>
                        </div>
                    
                        <!-- Start date input -->
                        <div class="form-outline mb-4">
                            {#if form_error.reservation_start_date != null}
                                <input type="date" id="form2Example2" class="form-control is-invalid" bind:value={new_reservation_data.reservation_start_date} on:input={dateChanged}/>
                                <div class="invalid-feedback">{form_error.reservation_start_date}</div>
                            {:else}
                                <input type="date" id="form2Example2" class="form-control" bind:value={new_reservation_data.reservation_start_date} on:input={dateChanged}/>
                            {/if}
                            <label class="form-label" for="form2Example2">Start Date</label>
                        </div>

                        <!-- End date input -->
                        <div class="form-outline mb-4">
                            {#if form_error.reservation_end_date != null}
                                <input type="date" id="form2Example2" class="form-control is-invalid" bind:value={new_reservation_data.reservation_end_date} on:input={dateChanged}/>
                                <div class="invalid-feedback">{form_error.reservation_end_date}</div>
                            {:else}
                                <input type="date" id="form2Example2" class="form-control" bind:value={new_reservation_data.reservation_end_date} on:input={dateChanged}/>
                            {/if}
                            <label class="form-label" for="form2Example2">End Date</label>
                        </div>                    
      
                        <!-- Umbrella id input -->
                        <div class="form-outline mb-4">
                            <select class="form-select" bind:value={new_reservation_data.reserved_umbrella_id}>
                                <option value={null}>Select the desired umbrella</option>
                                {#each free_umbrella as umbrella}
                                    <option value={umbrella}>
                                        {umbrella}
                                    </option>
                                {/each}
                            </select>
                            <label class="form-label" for="form2Example2">Umbrella number</label>
                            <!-- {#if form_error.reserved_umbrella_id}
                                <input type="number" id="form2Example2" class="form-control is-invalid" bind:value={new_reservation_data.reserved_umbrella_id}/>
                                <div class="invalid-feedback">{form_error.reserved_umbrella_id}</div>
                            {:else}
                                <input type="number" id="form2Example2" class="form-control" bind:value={new_reservation_data.reserved_umbrella_id}/>
                            {/if}
                            <label class="form-label" for="form2Example2">Umbrella number</label>-->
                        </div>  
                        
                        <!-- 2 column grid layout for inline styling -->
                        <div class="row mb-4">
                        <div class="col d-flex justify-content-center">
    
                        </div>
                    
                        </div>
                    
                        <!-- Submit button --> 
                        <button id="submit-button" type="button" class="btn btn-primary btn-block mb-4" on:click={postNewReservation}>Submit</button> 
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