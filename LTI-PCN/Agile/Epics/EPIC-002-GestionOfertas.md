# EPIC-002 — Gestión de Ofertas

## Descripción

Esta épica implementa el ciclo completo de creación, edición, revisión, aprobación y publicación de ofertas de trabajo (`JobPosting`). Incluye asistencia de IA para mejorar descripciones de rol y un flujo de revisión con hiring managers antes de publicar.

El objetivo es permitir que recruiters creen ofertas de alta calidad con menor esfuerzo manual y que hiring managers participen desde el inicio del proceso.

## Valor de Negocio

- **Reducción de tiempo de creación de ofertas**: IA ayuda a redactar y mejorar descripciones, ahorrando tiempo a recruiters.
- **Colaboración temprana con managers**: Flujo de revisión y aprobación antes de publicar asegura alineación y reduce retrabajos.
- **Calidad de contenido**: Ofertas mejor redactadas atraen candidatos más cualificados.
- **Base para automatización**: Las ofertas publicadas desencadenan automatizaciones (pipeline inicial, notificaciones).

## Funcionalidades Preliminares

- **Crear oferta**: Formulario con título, descripción, ubicación, tipo de contrato, requisitos, salario (opcional).
- **Asistencia de IA**: Sugerencia de mejora de descripción y resumen del rol a partir de inputs básicos.
- **Revisión y aprobación**: Enviar oferta a hiring manager para revisión; manager puede aprobar o solicitar cambios.
- **Publicar oferta**: Cambiar estado a "publicada" y activar automatizaciones iniciales.
- **Editar oferta**: Modificar oferta en estado borrador o publicada (con trazabilidad).
- **Cerrar oferta**: Marcar oferta como cerrada cuando se complete el proceso.
- **Listado de ofertas**: Vista filtrable por estado (borrador, publicada, cerrada) y por hiring manager.

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados y roles para asignar hiring managers.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 3-4 semanas (incluyendo integración con IA, flujo de aprobación y pruebas).

## Criterios de Éxito

- Un recruiter puede crear una oferta, solicitar mejora de IA, enviarla a revisión y publicarla tras aprobación del hiring manager.
- La oferta publicada queda visible para recibir candidaturas.
- Los cambios en la oferta quedan registrados en el historial.
