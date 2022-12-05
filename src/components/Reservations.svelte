<script lang="ts">
    import axios from "axios";
    import { onMount } from "svelte";
    import Login from "./Login.svelte";
    import Navbar from "./Navbar.svelte";
    const getCookieValue = (name: string) => (
      document.cookie.match('(^|;)\\s*' + name + '\\s*=\\s*([^;]+)')?.pop() || ''
    )
    let reservations = []
    onMount(async () =>{
        await axios.get('http://localhost:8000/api/v1/beachreservation/', {
                withCredentials: true
            }).then(response =>{
                console.log(response.data)
                reservations = response.data
            }).catch(error =>{
                console.log(error)
        })
    })
    let selected_reservation = {
        id: null,
        index: null,
    }
    async function deleteSelectedReservation(){
        await axios.delete(`http://localhost:8000/api/v1/beachreservation/${selected_reservation.id}/`,{
            headers: {
                'X-CSRFToken': getCookieValue('csrftoken'),
            },
            withCredentials: true,
        })
        .then((data) =>{
            document.getElementById('dismiss-button').click()
            reservations.splice(selected_reservation.index,1)
            // This triggers svelte update
            reservations = reservations
        })
        .catch((error) =>{
            console.log(error)
        });   
    }
</script>

<main>
    <Navbar />
    <h1>Reservation page</h1>
    <!-- Modal -->
    <div class="modal fade " id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="staticBackdropLabel">Are you sure you want to delete Reservation #{selected_reservation.id}?</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                    <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="dismiss-button">Close</button>
                    <button type="button" class="btn btn-danger" on:click={deleteSelectedReservation}>Delete</button>
                </div>
            </div>
        </div>
    </div>
    {#each reservations as reservation, i}
        <div class="card" id="{""+i}">
            <div class="card-body">
                <h5 class="card-title">Reservation #{reservation.id}</h5>
            </div>
            <ul class="list-group list-group-flush">
                <li class="list-group-item">Reservation period: {reservation.reservation_start_date} - {reservation.reservation_end_date}</li>
                <li class="list-group-item">Number of seats: {reservation.number_of_seats}</li>
                <li class="list-group-item">Reserved umbrella number: {reservation.reserved_umbrella_id}</li>
                <li class="list-group-item">Price: {reservation.reservation_price}€ </li>
            </ul>
            <!-- Button trigger modal -->
            <button on:click={() => {
                selected_reservation.id = reservation.id;
                selected_reservation.index = i
            }} type="button" class="btn btn-danger" data-bs-toggle="modal" data-bs-target="#staticBackdrop">Cancel reservation</button>
        </div>
    {/each}
</main>

<style>


</style>