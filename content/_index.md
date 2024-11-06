---
title: Hextra Theme
layout: hextra-home
---

{{< hextra/hero-badge >}}
  <div class="hx-w-2 hx-h-2 hx-rounded-full hx-bg-green-100"></div>
  <span>En línea</span>
  {{< icon name="arrow-circle-right" attributes="height=14" >}}
{{< /hextra/hero-badge >}}

<!-- {{< hextra/hero-badge >}}
  <div class="hx-w-2 hx-h-2 hx-rounded-full hx-bg-red-status"></div>
  <span>Fuera de servicio</span>
  {{< icon name="arrow-circle-right" attributes="height=14" >}}
{{< /hextra/hero-badge >}} -->

<div class="hx-mt-6 hx-mb-6">
{{< hextra/hero-headline >}}
  Documentación del &nbsp;<br class="sm:hx-block hx-hidden" />cluster HPC Khipu
{{< /hextra/hero-headline >}}
</div>

<div class="hx-mb-12">
{{< hextra/hero-subtitle >}}
  Guías de inicio rápido, uso de software, envío de jobs y más.
{{< /hextra/hero-subtitle >}}
</div>


<div class="hx-mt-6"></div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    link="/docs"
    title="Primeros Pasos"
    subtitle="Si no sabe por donde empezar con la documentación, este es un buen punto de partida."
    class="hx-aspect-auto"
    style="background: radial-gradient(ellipse at 50% 80%,rgba(194,97,254,0.15),hsla(0,0%,100%,0));"
  >}}
  {{< hextra/feature-card
    link="/docs/guia-de-usuario/iniciar-sesion"
    title="Guía de Usuario"
    subtitle="Información detallada sobre la política de uso y manejo del cluster."
    class="hx-aspect-auto"
    style="background: radial-gradient(ellipse at 50% 80%,rgba(142,53,74,0.15),hsla(0,0%,100%,0));"
  >}}
  {{< hextra/feature-card
    link="/docs/guia-de-usuario/software"
    title="Software"
    subtitle="Aquí encontrará la lista de software disponible, su uso y cómo se puede añadir nuevo software."
    class="hx-aspect-auto"
    style="background: radial-gradient(ellipse at 50% 80%,rgba(221,210,59,0.15),hsla(0,0%,100%,0));"
  >}}
    {{< hextra/feature-card
    link="/docs/guia-de-usuario/transferencia-de-datos/"
    title="Transferencia de archivos"
    subtitle="Aquí encontrará información sobre cómo transferir sus datos hacia y desde Khipu.."
    class="hx-aspect-auto"
    style="background: radial-gradient(ellipse at 50% 80%,rgba(194,97,254,0.15),hsla(0,0%,100%,0));"
  >}}
  {{< hextra/feature-card
    link="/"
    title="Tópicos Avanzados"
    subtitle="Información más detalla sobre el funcionamiento del cluster."
    class="hx-aspect-auto"
    style="background: radial-gradient(ellipse at 50% 80%,rgba(221,210,59,0.15),hsla(0,0%,100%,0));"
  >}}
  
{{< /hextra/feature-grid >}}