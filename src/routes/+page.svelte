<script>
  import {
    AccordionItem,
    Heading,
    P,
    A,
    Label,
    Alert,
    Checkbox,
    Card,
  } from "flowbite-svelte";
  import { ArrowDown, ArrowUp, Check } from "svelte-bootstrap-svg-icons";
  import data from "$lib/data.js";

  export const snapshot = {
    capture: () => list,
    restore: (value) => (list = value),
  };

  const listMemory = () =>
    data.sections.reduce(
      (acc, section) => ({
        ...acc,
        [section.title]: section.items.map(() => 0),
        [section.title + "bonus"]: section.bonus?.map(() => 0),
      }),
      {}
    );
  let list = listMemory();
</script>

<svelte:head>
  <title>Karštejn – Seznam věcí s sebou</title>
</svelte:head>

<main class="stack">
  <Heading>Co s sebou na Karštejn?</Heading>
  <div class="stack">
    <P>
      Máte strach z balení věcí? Vždy na něco zapomenete?<br />Projděte si náš
      chytrý seznam.
    </P>
    <P>
      Máte strach z balení věcí? Vždy na něco zapomenete?<br />Projděte si náš
      chytrý seznam. Aby měly děti sebou všechno potřebné, připravili jsme
      doporučený seznam věcí na dětský tábor
    </P>
    <P
      >Jednotlivé položky si můžete odškrtnout. Až se sem vrátíte, seznam si
      bude pamatovat, co už máte odškrtnuté.</P
    >
    <P>
      Máte radši v ruce papír nebo chcete seznam přibalit dětem s sebou? Seznam
      si můžete stahnout a vytisknout zde:
      <A
        href="https://docs.google.com/document/d/1ziAREXzcZgufkk7Pec6bIkuwzzUzDwYlzbf4Y1xWdIQ/edit?usp=sharing"
        >Co s sebou</A
      >
    </P>
    <!-- <P>
      K položkám se symbolem <b>~</b> najdete více info v kateogrii
      <b>Naše Tipy</b>
    </P> -->
  </div>

  <Heading tag="h2">Seznam</Heading>
  <div>
    {#each data.sections as section, sectionIndex}
      <AccordionItem id={sectionIndex}>
        <svelte:fragment slot="header">
          <Heading tag="h3">
            {section.title}
          </Heading>
          <P class="mr-4">
            {list[section.title].filter(Boolean).length}/{list[section.title]
              .length}
          </P>
        </svelte:fragment>
        <span slot="arrowup">
          <ArrowUp />
        </span>
        <span slot="arrowdown">
          <ArrowDown />
        </span>
        <div class="stack stack-4">
          {#each section.items as item, itemIndex}
            <Label>
              <Checkbox bind:checked={list[section.title][itemIndex]} />
              <span class="pl-2">{item}</span>
            </Label>
          {/each}
          <Heading tag="h5">Nepovinné</Heading>
          {#if section.bonus?.length}
            {#each section.bonus as bonus, bonusIndex}
              <Label>
                <Checkbox
                  bind:checked={list[section.title + "bonus"][bonusIndex]}
                />
                <span class="pl-2">{bonus}</span>
              </Label>
            {/each}
          {/if}
        </div>
      </AccordionItem>
    {/each}
  </div>

  <Heading tag="h2">Naše tipy</Heading>
  <div>
    <div class="cluster">
      <div style="align-items: flex-start;">
        {#each data.tips.items as tip, tipIndex}
          <Card>
            <Heading tag="h3">{tip.title}</Heading>
            <div class="stack">
              {#each tip.detail as p, itemIndex}
                <P>{@html p}</P>
              {/each}
            </div>
          </Card>
        {/each}
      </div>
    </div>
  </div>

  <Heading tag="h2">U autobusu</Heading>
  <div>
    <P>K rychlému odbavení dětí při odjezdu, si prosím připravte:</P>
    <ul class="list-disc">
      <li>Originál přihlášky</li>
      <li>Originál potvrzení od lékaře</li>
      <li>
        Potřebné léky, pokud je dítě po dobu tábora musí užívat společně s
        informacemi kolik, jak často a kdy (2x denně, ráno, večer, před jídlem,
        po jídle, ...)
      </li>
      <li>
        Průkaz zdravotní pojišťovny (<b>stačí kopie</b>)
      </li>
      <!-- <li>
      Čestné prohlášení o bezinfekčnosti. <b
        >Originál s podpisem a s datem začátku tábora:
      </b>
      <br />I. běh (<A
        href="https://docs.google.com/document/d/1bqhbdzMt6zAGex4PUUkVKN55W6OHB7EU3ViIvT-LDrM/edit?usp=sharing"
        >odkaz</A
      >)
      <br />II. běh (<A
        href="https://docs.google.com/document/d/1v6ecAEoKdgclalnzstZGMwh1lEYGuaIe0Xo-Q9-0fxs/edit?usp=sharing"
        >odkaz</A
      >)
    </li> -->
    </ul>
  </div>
  <Alert
    >Něco jsme zapomněli? Nevíte si rady nebo jen se chcete doptat? Neváhejte
    se na nás obrátit. Rádi vám pomůžeme.</Alert
  >
  <ul>
    <li><A href="tel:+420604405795">+420 604 405 795</A> (Jáchym Janoušek)</li>
    <li><A href="mailto:info(at)karstejn.org">info(at)karstejn.org</A> (Schránku kontrolujeme zpravidla 2x týdne)</li>
    <li><A href="https://www.karstejn.org">www.karstejn.org</A></li>
    <li><A href="https://www.facebook.com/LTKarstejn">facebook</A></li>
    <li><A href="https://www.instagram.com/karstejn">instagram</A></li>
  </ul>
</main>

<style lang="scss" global>
  body {
    padding: 3rem 1rem;
  }
  main {
    max-width: 800px;
    margin: 0 auto;
  }
</style>
