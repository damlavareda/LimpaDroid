# LimpaDroid — limpeza segura para Android

**Aplicativo nativo para Android 11+** inspirado na categoria de ferramentas do CCleaner, mas com nome e interface originais. Projeto Android Studio em Java, sem SDK de anúncios, sem internet e sem bibliotecas externas.

## Funcionalidades

- Painel com armazenamento usado/disponível e porcentagem real, via `StatFs`.
- Análise das fotos, vídeos e áudios **a que o usuário concedeu acesso**, via `MediaStore`.
- Localização de **arquivos grandes** (mídias com 50 MB ou mais).
- Localização de **capturas de tela** por pasta e/ou nome.
- Identificação de **cópias exatamente iguais**, comparando tamanho e SHA-256 integral. Mantém a cópia mais antiga fora da lista de remoção sugerida. Limites de custo: compara arquivos com até 512 MB e lê até 2 GB de candidatos por análise.
- Seleção manual e dupla confirmação para exclusão de mídias (`MediaStore.createDeleteRequest` com confirmação do Android).
- Leitura de **pasta que o usuário escolher**, inclusive documentos e downloads, via `ACTION_OPEN_DOCUMENT_TREE` / `DocumentsContract` (limite 2.500 arquivos e 8 níveis); exclusão de documentos apenas se o provedor permitir e se o usuário confirmar.
- Limpeza do **cache do próprio LimpaDroid** e atalho para o gerenciamento de armazenamento do Android.
- Interface escura em português, sem anúncios e sem transmissão de arquivos.

## Como compilar e instalar

1. Instale **Android Studio** recente que aceite AGP 8.13.2 e Gradle 8.13.
2. Em **File > Open**, selecione a pasta `LimpaDroid` extraída do ZIP.
3. Aceite instalar o Android SDK 36 e sincronizar o Gradle (necessita internet apenas no PC durante a compilação).
4. Conecte o celular com **Depuração USB** ativada, escolha o aparelho em Android Studio e pressione **Run**.
5. Para gerar APK: **Build > Build Bundle(s) / APK(s) > Build APK(s)**. O arquivo estará em `app/build/outputs/apk/debug/app-debug.apk`.
6. Instale o APK no celular (pode pedir autorização para instalar apps dessa origem).

**Versões:** Android 11 a Android 16+ (minSdk 30, compileSdk 36, targetSdk 35). Não foi possível compilar e executar neste ambiente, pois o SDK Android/Gradle não estão instalados; o projeto deve ser compilado no Android Studio.

## Limitações reais do sistema

- Não limpa o cache privado de outros apps, RAM nem dados do WhatsApp sem acesso específico; root não é solicitado.
- Em Android 14+, o usuário pode liberar **somente fotos selecionadas**, e a análise respeita esse limite. Permissões de imagens, vídeos e áudios são independentes.
- Não inclui acesso irrestrito `MANAGE_EXTERNAL_STORAGE`, que tem política restrita no Google Play.
- A exclusão é permanente. Revise os itens antes de confirmar. Para mídias, o Android apresenta sua própria autorização; para documentos de uma pasta concedida, o app apresenta um diálogo de confirmação.
- A análise de duplicados foca em mídia e não cobre automaticamente arquivos fora das coleções visíveis; análise de pasta exibe itens por tamanho, sem deduplicação.
- O indicador "utilizado" mede o volume interno reportado pelo sistema, não o tamanho total de arquivos que o aplicativo consegue ler.

## Arquivos principais

- `app/src/main/java/br/com/limpadroid/MainActivity.java`: interface, navegação, seleção e exclusão confirmada.
- `MediaScanner.java`: consulta mídia e SHA-256 de duplicados.
- `FolderScanner.java`: percorre pasta liberada por SAF.
- `CacheCleaner.java`: remove somente o cache próprio.
- `AndroidManifest.xml`: permissões granulares e nenhuma permissão de rede.

## Privacidade

Nenhuma permissão de Internet é declarada. O aplicativo não inclui analytics, trackers ou anúncios. O escaneamento ocorre localmente, sem upload dos arquivos.

## Referências técnicas

- Android MediaStore: https://developer.android.com/reference/android/provider/MediaStore
- Android shared media: https://developer.android.com/training/data-storage/shared/media
- Storage Access Framework: https://developer.android.com/guide/topics/providers/document-provider
- Android 14 Selected Photos Access: https://developer.android.com/about/versions/14/behavior-changes-14
- Política de All files access do Google Play: https://support.google.com/googleplay/android-developer/answer/10467955?hl=pt-br
