<script lang="ts">
  import { link } from 'svelte-spa-router'
  import type { BookItem } from '@/repositories/book'
  /** Props */
  export let book: BookItem

  // NOTE: $: is LABEL. 
  //       It will be recalculated every time the variable is updated.
  $: src = book.volumeInfo.imageLinks 
    ? book.volumeInfo.imageLinks.smallThumbnail
    : 'https://placehold.jp/bcbcc2/ffffff/160x120.png?text=No%20image'
  
  $: description = book.volumeInfo.description
    ? `${book.volumeInfo.description.slice(0, 100)}...`
    : ''
</script>

<div class="w-full sm:flex">
  <div
    class="
      h-96
      sm:h-auto
      sm:w-48
      flex-none
      bg-cover
      rounded-t
      sm:rounded-t-none
      sm:rounded-l
      text-center
      overflow-hidden
    "
    style={`background-image: url('${src}')`}
  >
  </div>
  <div class="
    border-r
    border-b
    border-l
    border-grey-light
    sm:border-l-0
    sm:border-t
    sm:border-grey-light
    bg-white
    rounded-b
    sm:rounded-b-none
    sm:rounded-r
    p-4
    flex
    flex-col
    justify-between
    leading-normal
    w-100
    sm:w-9/12
    lg:w-7/12
  ">
    <div class="my-4">
      <a href={`/books/${book.id}`} use:link>
        <div class="text-black font-bold text-xl mb-2">{book.volumeInfo.title}</div>
      </a>
      <p class="text-grey-darker text-sm break-words w-9/12 m-auto">
        {description}
      </p>
    </div>
  </div>
</div>