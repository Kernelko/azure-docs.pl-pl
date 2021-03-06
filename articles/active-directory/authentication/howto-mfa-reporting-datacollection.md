---
title: Zbieranie danych użytkownika w usłudze Azure Multi-Factor Authentication
description: Jakie informacje umożliwiają uwierzytelnianie użytkowników przez usługę Azure Multi-Factor Authentication?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 1b380bc20c9f80710ca62672b99649ce3498a8e8
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223766"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Zbieranie danych użytkownika w usłudze Azure Multi-Factor Authentication

W tym dokumencie wyjaśniono, jak znaleźć informacje o użytkowniku zebrane przez serwer usługi Azure Multi-Factor Authentication (MFA Server) i usługi Azure MFA (oparte na chmurze), w przypadku, gdy chcesz go usunąć.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Zebrane informacje

Serwer MFA, rozszerzenia serwera NPS i systemu Windows Server 2016 usługi Azure MFA adaptera AD FS zbierać i przechowywać następujące informacje w ciągu 90 dni.

Próby uwierzytelnienia (używane do raportowania i rozwiązywania problemów):

- Sygnatura czasowa
- Nazwa użytkownika
- Imię
- Nazwisko
- Adres e-mail
- Grupa użytkownika
- Metoda uwierzytelniania (połączenie telefoniczne, Token OATH komunikatów, aplikacja mobilna tekst)
- Tryb połączenia telefonicznego (standardowy, PIN)
- Kierunek wiadomości tekstowej (jednokierunkowe, dwukierunkowe)
- Tryb wiadomości tekstowej (OTP, uwierzytelnianie OTP + kod PIN)
- Tryb aplikacji mobilnej (standardowy, PIN)
- Tryb tokenu OATH (standardowy, PIN)
- Typ uwierzytelnienia
- Nazwa aplikacji
- Wywołanie podstawowy numer kierunkowy kraju
- Wywołanie podstawowego numeru telefonu
- Rozszerzenie podstawowego wywołania
- Podstawowe wywołanie uwierzytelniony
- Wynik wywołania podstawowego
- Numer kierunkowy kraju wywołania kopii zapasowej
- Numer telefonu wywołania kopii zapasowej
- Rozszerzenie kopii zapasowej wywołania
- Uwierzytelnione wywołania kopii zapasowej
- Wynik wywołania kopii zapasowej
- Ogólna uwierzytelniony
- Ogólny wynik
- Wyniki
- Uwierzytelniony
- Wynik
- Inicjowanie adresu IP
- Urządzenia
- Token urządzenia
- Typ urządzenia
- Wersja aplikacji mobilnej
- Wersja systemu operacyjnego
- Wynik
- Sprawdzanie używane dla powiadomień

Aktywacje (próby aktywacji konta w aplikacji mobilnej Microsoft Authenticator):
- Nazwa użytkownika
- Nazwa konta
- Sygnatura czasowa
- Pobierz wynik kod aktywacji
- Aktywuj sukces
- Błąd aktywacji
- Wynik stanu aktywacji
- Nazwa urządzenia
- Typ urządzenia
- Wersja aplikacji
- Token OATH, włączone

Bloki (używany do określenia stan blokady oraz raportowanie):

- Blokuj znacznik czasu:
- Blok według nazwy użytkownika
- Nazwa użytkownika
- Kod kraju
- Numer telefonu
- Numer telefonu w formacie
- Wewnętrzny
- Czyszczenie rozszerzenia
- Zablokowany
- Przyczyna blokady
- Sygnatura czasowa zakończenia
- Przyczyna zakończenia
- Blokada konta
- Alert oszustwa
- Alert oszustwa nie zostało zablokowane
- Język

Pomija (używane na potrzeby raportowania):

- Sygnatura czasowa obejścia
- Liczba sekund obejścia
- Obejście według nazwy użytkownika
- Nazwa użytkownika
- Kod kraju
- Numer telefonu
- Numer telefonu w formacie
- Wewnętrzny
- Czyszczenie rozszerzenia
- Przyczyna obejścia
- Sygnatura czasowa zakończenia
- Przyczyna zakończenia
- Obejście używane

Zmiany (używane do synchronizacji zmiany użytkowników serwera usługi MFA lub usługi AAD):

- Zmiana sygnatury czasowej
- Nazwa użytkownika
- Nowy kod kraju
- Nowy numer telefonu
- Nowy numer wewnętrzny
- Nowe zapasowy numer kierunkowy kraju
- Nowe zapasowego numeru telefonu
- Nowe rozszerzenie kopii zapasowej
- Nowy kod PIN
- Wymagana zmiana numeru PIN
- Stary Token urządzenia
- Token nowego urządzenia

## <a name="gather-data-from-mfa-server"></a>Zbieranie danych z serwera MFA

Następujący proces serwera usługi MFA w wersji 8.0 lub nowszej umożliwia Administratorzy mogą wyeksportować wszystkie dane dla użytkowników:

- Zaloguj się do serwera usługi MFA, przejdź do folderu **użytkowników** karty, wybierz użytkownika, a następnie kliknij przycisk **Edytuj** przycisku. Zrób zrzuty ekranu (Alt-PrtScn), każdą kartę, aby udostępniać użytkownikom ich bieżących ustawień usługi MFA.
- Z serwera usługi MFA w wierszu polecenia wpisz następujące polecenie Zmiana ścieżki zgodnie z instalacji `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` aby wygenerować JSON w formacie pliku.
- Administratorzy mogą również użyć operacji GetUserGdpr zestawu SDK usługi sieci Web jako opcja do wyeksportowania informacji usługi chmury usługi MFA w wszystkie zebrane dla danego użytkownika lub dołączyć do większych rozwiązanie do raportowania.
- Wyszukiwanie `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` i wszelkich kopii zapasowych dla "<username>" (z uwzględnieniem cudzysłowów w wyszukiwaniu) aby znaleźć wszystkie wystąpienia elementu rekord użytkownika jest dodane lub zmienione.
   - Te rekordy mogą być ograniczone (ale nie eliminuje), usuwając zaznaczenie pola **"Log zmiany wprowadzane przez użytkownika"** w Interfejsie serwera MFA, sekcja rejestrowanie, karta pliki dziennika.
   - Skonfigurowanie usługi syslog i **"Log zmiany wprowadzane przez użytkownika"** zaznaczono w Interfejsie serwera MFA, sekcja rejestrowanie, karta dziennik systemowy, a następnie wpisy dziennika można gromadzić z syslog zamiast tego.
- Inne zdarzenia, nazwy użytkownika w dziennikach MultiFactorAuthSvc.log i inny serwer MFA logowania pliki należące do uwierzytelniania prób są traktowane jak działa i jest niezamawianymi podany przy użyciu eksportu MultiFactorAuthGdpr.exe lub zestawu SDK usługi sieci Web GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>Usuwanie danych z serwera MFA

Z serwera usługi MFA w wierszu polecenia wpisz następujące polecenie Zmiana ścieżki zgodnie z instalacji `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` można usunąć wszystkich MFA informacji usługi chmury zbieranych dla tego użytkownika.

- Dane zawarte w danych eksportu zostanie usunięty w czasie rzeczywistym, ale może potrwać do 30 dni dla danych operacyjnych lub niezamawianymi ma zostać całkowicie usunięty.
- Administratorzy mogą również służy operacja DeleteUserGdpr zestawu SDK usługi sieci Web jako opcja do usuwania informacji usługi chmury usługi MFA w wszystkie zebrane dla danego użytkownika lub dołączyć do większych rozwiązanie do raportowania.

## <a name="gather-data-from-nps-extension"></a>Zbieranie danych z rozszerzenia serwera NPS

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) Wyślij żądanie eksportu.

- Uwierzytelnianie wieloskładnikowe informacje znajdują się w pliku eksportu, co może potrwać, godzin lub dni, aby zakończyć.
- Wystąpienia nazwy użytkownika w dziennikach zdarzeń AzureMfa/AuthZ/AuthZOptCh AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh i są traktowane jako operacyjne i niezamawianymi z informacji podanych w danych eksportu.

## <a name="delete-data-from-nps-extension"></a>Usuwanie danych z rozszerzenia serwera NPS

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) żądania dla konta Zamknij, aby usunąć wszystkie MFA informacji usługi chmury zbieranych dla tego użytkownika.

- Może potrwać do 30 dni dla danych ma zostać całkowicie usunięty.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Zbieranie danych z systemu Windows Server 2016 usługi Azure MFA adaptera AD FS

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) Wyślij żądanie eksportu. 

- Uwierzytelnianie wieloskładnikowe informacje znajdują się w pliku eksportu, co może potrwać, godzin lub dni, aby zakończyć.
- Wystąpienia nazwy użytkownika w dziennikach zdarzeń śledzenia/Debug dla usługi AD FS (jeśli jest włączona) są uznawane za operacyjne i niezamawianymi z informacji podanych w danych eksportu.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Usuwanie danych z systemu Windows Server 2016 usługi Azure MFA adaptera AD FS

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) żądania dla konta Zamknij, aby usunąć wszystkie MFA informacji usługi chmury zbieranych dla tego użytkownika.

- Może potrwać do 30 dni dla danych ma zostać całkowicie usunięty.

## <a name="gather-data-for-azure-mfa"></a>Zbieranie danych dla usługi Azure MFA

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) Wyślij żądanie eksportu.

- Uwierzytelnianie wieloskładnikowe informacje znajdują się w pliku eksportu, co może potrwać, godzin lub dni, aby zakończyć.

## <a name="delete-data-for-azure-mfa"></a>Usuwanie danych dla usługi Azure MFA

Użyj [Portal ochrony prywatności firmy Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) żądania dla konta Zamknij, aby usunąć wszystkie MFA informacji usługi chmury zbieranych dla tego użytkownika.

- Może potrwać do 30 dni dla danych ma zostać całkowicie usunięty.

## <a name="next-steps"></a>Kolejne kroki

[Serwer MFA raportowania](howto-mfa-reporting.md)
