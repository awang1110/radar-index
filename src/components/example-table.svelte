<script>

    import { getBibtexEntries } from "../util.js";

    let sortAsc = true;
    let sortKey = "title"
    let lastSortKey = ""

    let examples = getBibtexEntries()

    let sortIcon;
    function updateSortIcon() {
        if (sortAsc) {
            sortIcon = "arrow_downward"
        } else {
            sortIcon = "arrow_upward"
        }
    }

    function sort() {
        // if sorting by different column, always sort ascending
        if (lastSortKey !== sortKey) {
            sortAsc = true
            examples.sort((a,b) => (a.entryTags[sortKey] > b.entryTags[sortKey]) ? 1 : ((b.entryTags[sortKey] > a.entryTags[sortKey]) ? -1 : 0));
        } else {
            if (sortAsc) {
                sortAsc = false
                examples.sort((a,b) => (a.entryTags[sortKey] > b.entryTags[sortKey]) ? -1 : ((b.entryTags[sortKey] > a.entryTags[sortKey]) ? 1 : 0));
            } else {
                sortAsc = true
                examples.sort((a,b) => (a.entryTags[sortKey] > b.entryTags[sortKey]) ? 1 : ((b.entryTags[sortKey] > a.entryTags[sortKey]) ? -1 : 0));
            }
        }
        examples = examples
        updateSortIcon()
    }

    // initial sort on load
    sort()

</script>

<style>

    td a {
        color: orange;
        font-weight: bold;
    }

    #example-table-wrapper {
        grid-column: page;
    }

    table {
          /* border-collapse: collapse;  */
          height: 500px;
          display: block;
          overflow: scroll;
          border-top: 1px solid rgba(0, 0, 0, 0.2);
    }

    /* thead {
        position: sticky;
    } */

    tbody {
        /* overflow: scroll;
        display: block;
        height: 500px; */
    }

    th:hover {
        cursor: pointer;
    }

    i {
        vertical-align: -4px; /* hacky, center icons with table header */
        font-size: 18px;
    }

    th {
        white-space: nowrap;
    }

    #title-col {
        width: 800px;
    }

    /* tbody > tr:hover {
        background-color: #efefef;
    } */

    /* a {
        border-bottom: none;
    } */

    /* tr:nth-child(even) {
        background-color: #f2f2f2;
    } */

    /* td {
        border: none
    } */

    /* .link-icon:hover {
        color: var(--orange);
    } */

    .break { display: none; }
    .hide-column { 
      display: auto; 
      width: 200;
    }

    figure {
		margin-bottom: 1.5em;
    }
    
    @media(max-width: 1000px) {
        th { font-size: 1em !important }
        
        td, tr {
            font-size: 0.85em !important;
            line-height: 1.35em;
        }

        .break { display: inline-block; }
        .hide-column { display: none; }
    }

    th {
        border-bottom: 2px solid rgba(0, 0, 0, 0.1);
    }

    tr {
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    }

    td {
        padding: 8px;
        vertical-align: top;
        border: none;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    }
</style>

<figure class="subgrid">
    <div id="example-table-wrapper">
        <table id="example-table">
            <thead>
                <tr>
                <th class="hide-column" scope="col" on:click={() => {lastSortKey = sortKey; sortKey = "tags"; sort()}}>分類 <i class="material-icons">{sortKey === "tags" ? sortIcon : ""}</i></th>
                <th id="title-col" scope="col" on:click={() => {lastSortKey = sortKey; sortKey = "title"; sort()}}>每日新聞 <i class="material-icons">{sortKey === "title" ? sortIcon : ""}</i></th>
                </tr>
            </thead>
            <tbody>
                {#each examples as example}
                    <tr>
                        <td class="hide-column">{example.entryTags.tags}</td>
                        <td style="text-transform: capitalize">
                            <!-- <a href={example.entryTags.url}>{example.entryTags.title}</a> -->
                            <a>{example.entryTags.title}</a>
                        </td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</figure>
