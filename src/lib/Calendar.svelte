<script>
  import dayjs from 'dayjs'

  const DatesToShow = 9;
  const offset =  Math.floor(DatesToShow / 2);

  let selectedDate = dayjs().date();
  let selectedMonth = dayjs().month() + 1;
  let Dates = displayDates();

  function displayDates(){
    return Array.from({length: DatesToShow}, (_, i) => {
      const provisionalDate = selectedDate + i - offset;
      const isSelected = provisionalDate === selectedDate

      const month = provisionalDate < 1 ? selectedMonth - 1 : selectedMonth;
      if (provisionalDate < 1) {
        const date = dayjs().set('month', month).endOf('month').date() + provisionalDate;        
        return {
          str: getDateString(date, month),
          isSelected
        }

      }

      const lastDateOfMonth = dayjs().endOf('month').date();
      if (provisionalDate > lastDateOfMonth){
        const date = provisionalDate - lastDateOfMonth;
        return {
          str: getDateString(date, month),
          isSelected
        }
      }

      return {
        str: getDateString(provisionalDate, month),
        isSelected
      }
    });
  }

  function getDateString(date, month){
    return `${String(date).padStart(2, '0')}/${String(month).padStart(2, '0')}`
  }

  function selectDate(event) {
    [selectedDate, selectedMonth] = event.target.innerText.split('/');
    selectedDate = Number(selectedDate)
    selectedMonth = Number(selectedMonth)
    Dates = displayDates();
  }

</script>

<div>
  <div class="row">
    <ul class="dates">
      {#each Dates as date}
        <li class="date-item {date.isSelected ? 'selected-date': ''}" on:click={selectDate}>{date.str}</li>
      {/each}
    </ul>
  </div>
</div>


<style>
  .dates {
    list-style: none;
  }

  .date-item{
    display:  inline;

    border-radius: 1rem;
    background-color: rgb(162, 0, 0);

    color: white;

    padding: .5rem;
    margin-right: .75rem; 
  }

  .selected-date {
    background-color: rgb(42, 73, 165);
  }
</style>