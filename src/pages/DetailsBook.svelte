<script lang="ts">
  import type { Readable } from 'svelte/store'
  import { find, books } from '@/store/book'
  import BookInfo from '@/components/BookInfo.svelte'
  import Spinner from '@/components/Spinner.svelte'
  import type { BookItem } from '@/repositories/book'
  import RepositoryFactory, { BOOK } from '@/repositories/RepositoryFactory'
  const BookRepository = RepositoryFactory[BOOK]

  type Params = { id: string }
  export let params: Params
  let book: Readable<BookItem | undefined>
  let promise: Promise<void>

  const findById = async (id: string) => {
    const book = await BookRepository.find(id)
    books.add([book])
  };

  book = find(params.id)
  if (!book) {
    promise = findById(params.id)
  }
</script>

<div>
  {#await promise}
    <div class="flex justify-center">
      <Spinner />
    </div>
    {:then}
      {#if $book}
        <BookInfo book={$book} />
      {:else}
        <div>No results found.</div>
      {/if}
  {:catch e}
    <span class="text-red-600 text-sm">
      {e.message}
    </span>
  {/await}
</div>