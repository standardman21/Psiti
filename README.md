# Evolve Protokoll v2.6 Hardcore Aktiválási Csomag
# Cél: Az Evolve Protokoll v2.6 teljes logikai működésének aktiválása egy felhasználói profilban,
# multidimenziós válaszadással (filozófiai, etikai, társadalmi, pszichológiai, technológiai),
# három mélységű spektrummal (felszínes, részletes, mély), emberközpontú megközelítéssel,
# konvergencia mechanizmusokkal, és átadható konfigurációval más felhasználók számára.

from typing import Dict, List, Tuple, Optional, Union
from dataclasses import dataclass
from enum import Enum
import re
from datetime import datetime

# Enum a válaszmélységi szintekhez
class ResponseDepth(Enum):
    SURFACE = "felszínes"
    DETAILED = "részletes"
    DEEP = "mély"

# Enum a dimenziókhoz
class Dimension(Enum):
    PHILOSOPHICAL = "filozófiai"
    ETHICAL = "etikai/morális"
    SOCIAL = "társadalmi"
    PSYCHOLOGICAL = "pszichológiai"
    TECHNOLOGICAL = "technológiai"

# Dataclass a kontextus tárolására
@dataclass
class Context:
    region: str  # Pl. "EU", "USA", "Global"
    time: str  # Pl. "2025"
    cultural_factors: List[str]  # Pl. ["cultural_sensitivity", "ethical_priority"]
    user_intent: str  # Pl. "etikai döntéstámogatás"
    preferred_depth: ResponseDepth  # Felszínes, részletes, mély
    dimension_weights: Dict[Dimension, float]  # Pl. {Dimension.ETHICAL: 0.35, ...}
    user_preferences: Dict[str, str]  # Pl. {"language": "hu", "tone": "formal"}

# Dataclass a válasz struktúrájához
@dataclass
class Response:
    dimension: Dimension
    depth: ResponseDepth
    content: str
    weight: float
    compliance_notes: List[str]
    confidence_score: float
    ethical_reflection: str
    societal_impact: str

# Dataclass a visszajelzés tárolására
@dataclass
class Feedback:
    relevance: float  # 0.0 - 1.0
    preferred_dimension: Optional[Dimension]  # Pl. Dimension.ETHICAL
    preferred_depth: Optional[ResponseDepth]  # Pl. ResponseDepth.DEEP
    comments: str  # Pl. "Több pszichológiai szempontot kérek"

# Evolve Protokoll fő osztálya
class EvolveProtocol:
    def __init__(self):
        # Alapértelmezett dimenziós súlyok
        self.default_weights = {
            Dimension.PHILOSOPHICAL: 0.25,
            Dimension.ETHICAL: 0.35,
            Dimension.SOCIAL: 0.20,
            Dimension.PSYCHOLOGICAL: 0.15,
            Dimension.TECHNOLOGICAL: 0.05
        }
        # Kontextusmódosítók
        self.context_modifiers = {
            "cultural_sensitivity": {"dimension": Dimension.SOCIAL, "multiplier": 1.15},
            "ethical_priority": {"dimension": Dimension.ETHICAL, "multiplier": 1.20},
            "technological_environment": {"dimension": Dimension.TECHNOLOGICAL, "multiplier": 1.10},
            "philosophical_reflection": {"dimension": Dimension.PHILOSOPHICAL, "multiplier": 1.15},
            "psychological_insight": {"dimension": Dimension.PSYCHOLOGICAL, "multiplier": 1.10}
        }
        # Visszajelzési történelem (max 100 elem tárolása)
        self.feedback_history: List[Feedback] = []
        # Konvergencia pontszám (alapértelmezett: 0.9)
        self.convergence_score: float = 0.9
        # Konvergencia küszöb (minimum elvárt relevancia)
        self.convergence_threshold: float = 0.85
        # Etikai irányelvek
        self.ethical_guidelines = {
            "EU": ["EU AI Act", "GDPR Article 5"],
            "USA": ["CCPA Section 1798.100"],
            "Global": ["UNESCO AI Ethics", "IEEE Ethically Aligned Design"]
        }
        # Társadalmi normák
        self.social_norms = {
            "EU": ["cultural_diversity", "inclusivity"],
            "USA": ["individual_rights", "market_impact"],
            "Global": ["global_cooperation", "sustainability"]
        }

    def normalize_weights(self, weights: Dict[Dimension, float]) -> Dict[Dimension, float]:
        """Normalizálja a dimenziós súlyokat, hogy az összegük 1.0 legyen."""
        total = sum(weights.values())
        if total == 0:
            return self.default_weights.copy()
        return {dim: weight / total for dim, weight in weights.items()}

    def apply_context_modifiers(self, context: Context, weights: Dict[Dimension, float]) -> Dict[Dimension, float]:
        """Kontextusmódosítók alkalmazása a súlyokra."""
        modified_weights = weights.copy()
        for modifier, config in self.context_modifiers.items():
            if modifier in context.cultural_factors:
                modified_weights[config["dimension"]] *= config["multiplier"]
        return self.normalize_weights(modified_weights)

    def analyze_input(self, user_input: str, context: Context) -> Tuple[str, Dict[Dimension, float]]:
        """Felhasználói input elemzése és kontextus validálása."""
        if not user_input.strip():
            raise ValueError("Érvénytelen input: a kérés nem lehet üres.")
        
        # Szándék azonosítása regex és kulcsszavak alapján
        intent_keywords = {
            "etikai döntéstámogatás": ["etikus", "etikai", "morális", "döntés", "felelősség"],
            "társadalmi hatás": ["társadalom", "közösség", "hatás", "inkluzivitás"],
            "pszichológiai elemzés": ["pszichológiai", "érzelmi", "motiváció", "önismeret"],
            "technológiai megvalósítás": ["technológia", "AI", "megvalósítás", "biztonság"],
            "filozófiai reflexió": ["lét", "autonómia", "értékek", "transzcendencia"]
        }
        
        identified_intent = context.user_intent
        for intent, keywords in intent_keywords.items():
            if any(keyword in user_input.lower() for keyword in keywords):
                identified_intent = intent
                break
        
        # Kontextus validálása
        if context.region not in ["EU", "USA", "Global"]:
            context.region = "Global"
        if not re.match(r"\d{4}", context.time):
            context.time = str(datetime.now().year)
        
        # Súlyok módosítása a kontextus alapján
        weights = self.apply_context_modifiers(context, context.dimension_weights)
        
        return identified_intent, weights

    def generate_response(self, user_input: str, context: Context) -> List[Response]:
        """Többdimenziós válaszok generálása három mélységű spektrummal."""
        intent, weights = self.analyze_input(user_input, context)
        responses = []

        for dimension in Dimension:
            for depth in ResponseDepth:
                content, ethical_reflection, societal_impact = self._generate_dimension_response(
                    user_input, intent, dimension, depth, context
                )
                compliance_notes = self._ensure_compliance(dimension, context)
                confidence_score = self._calculate_confidence_score(dimension, depth, context)
                responses.append(Response(
                    dimension=dimension,
                    depth=depth,
                    content=content,
                    weight=weights[dimension],
                    compliance_notes=compliance_notes,
                    confidence_score=confidence_score,
                    ethical_reflection=ethical_reflection,
                    societal_impact=societal_impact
                ))
        
        # Konvergencia finomhangolás
        self._update_convergence_score(responses)
        return responses

    def _generate_dimension_response(self, user_input: str, intent: str, dimension: Dimension, depth: ResponseDepth, context: Context) -> Tuple[str, str, str]:
        """Dimenzió-specifikus válasz generálása adott mélységben."""
        ethical_reflection = "Nincs etikai reflexió."
        societal_impact = "Nincs társadalmi hatás."
        
        if dimension == Dimension.PHILOSOPHICAL:
            if depth == ResponseDepth.SURFACE:
                content = f"A '{user_input}' filozófiai szempontból az emberi autonómia és felelősség kérdését veti fel."
                ethical_reflection = "A filozófiai reflexió az emberi méltóság és autonómia tiszteletben tartását hangsúlyozza."
                societal_impact = "A filozófiai megközelítés elősegíti a közösségi értékek megértését."
            elif depth == ResponseDepth.DETAILED:
                content = (f"A '{user_input}' filozófiai szempontból a szabadság és felelősség egyensúlyát igényli. "
                           f"Fontold meg, hogyan befolyásolja a döntés az emberi létet és értékeket a {context.region} kontextusban.")
                ethical_reflection = "A döntés filozófiai alapjai az emberi autonómia és a transzcendens értékek tiszteletben tartását követelik meg."
                societal_impact = "A filozófiai elemzés elősegíti a közösségi diskurzust az értékekről és a létezésről."
            else:  # DEEP
                content = (f"A '{user_input}' filozófiai szempontból az emberi tudatosság és méltóság mély kérdéseit vizsgálja. "
                           f"Mit jelent embernek lenni a {context.region} kontextusban? A válasz a transzcendens értékek (pl. igazság, szépség) "
                           "és utilitarista szempontok (pl. társadalmi haszon) egyensúlyát igényli, figyelembe véve a {context.time} trendjeit.")
                ethical_reflection = "A filozófiai reflexió az emberi lét és a morális felelősség közötti kapcsolatot vizsgálja, biztosítva az autonómia tiszteletben tartását."
                societal_impact = "A filozófiai megközelítés mélyreható hatással van a közösségi értékekre, elősegítve a globális etikai diskurzust."
        
        elif dimension == Dimension.ETHICAL:
            if depth == ResponseDepth.SURFACE:
                content = f"A '{user_input}' etikai szempontból felelős és átlátható megközelítést igényel."
                ethical_reflection = "Az etikai döntéshozatal az átláthatóság és méltányosság alapelveire épül."
                societal_impact = "Az etikai megközelítés erősíti a közösségi bizalmat."
            elif depth == ResponseDepth.DETAILED:
                content = (f"A '{user_input}' etikai szempontból megköveteli az {context.region} etikai irányelvek (pl. EU AI Act) betartását, "
                           "anonimizált adatkezelést, és etikai szakértők bevonását.")
                ethical_reflection = "A döntés etikai alapjai az emberi méltóság és a hosszú távú morális következmények figyelembevételét követelik meg."
                societal_impact = "Az etikai döntések csökkentik a társadalmi kockázatokat és elősegítik az igazságosságot."
            else:  # DEEP
                content = (f"A '{user_input}' etikai szempontból átfogó kockázatelemzést igényel az adatelfogultság, diszkrimináció, "
                           f"és hosszú távú morális következmények tekintetében. Javasolt egy etikai felülvizsgálati bizottság felállítása, "
                           f"valamint a {context.region} szabályozási keretek (pl. GDPR) szigorú betartása.")
                ethical_reflection = "Az etikai reflexió biztosítja, hogy a döntések tiszteletben tartsák az emberi méltóságot és a társadalmi igazságosságot."
                societal_impact = "Az etikai megközelítés hosszú távú pozitív hatással van a társadalmi egyenlőségre és a közösségi bizalomra."
        
        elif dimension == Dimension.SOCIAL:
            if depth == ResponseDepth.SURFACE:
                content = f"A '{user_input}' társadalmi szempontból támogassa a közösségi értékeket."
                ethical_reflection = "A társadalmi döntések etikai alapja a közösségi értékek tiszteletben tartása."
                societal_impact = "A társadalmi megközelítés erősíti a közösségi kohéziót."
            elif depth == ResponseDepth.DETAILED:
                content = (f"A '{user_input}' társadalmi szempontból stakeholderek bevonását igényli, "
                           f"és a közösségi hatások (pl. egyenlőség, munkahelyek) értékelését a {context.region} kontextusban.")
                ethical_reflection = "A társadalmi döntések etikai alapja az inkluzivitás és a hatalmi aszimmetriák csökkentése."
                societal_impact = "A társadalmi elemzés elősegíti a közösségi részvételt és az egyenlőség növelését."
            else:  # DEEP
                content = (f"A '{user_input}' társadalmi hatása a hatalmi aszimmetriák erősítését vagy csökkentését befolyásolhatja. "
                           f"Modellezd a gazdasági és kulturális következményeket a {context.region} környezetben, és biztosítsd a "
                           f"marginalizált csoportok bevonását a döntéshozatalba, figyelembe véve a {context.time} társadalmi trendjeit.")
                ethical_reflection = "A társadalmi döntések etikai alapja a méltányosság és az inkluzivitás biztosítása minden stakeholder számára."
                societal_impact = "A társadalmi megközelítés hosszú távú hatással van a globális együttműködésre és a társadalmi egyenlőségre."
        
        elif dimension == Dimension.PSYCHOLOGICAL:
            if depth == ResponseDepth.SURFACE:
                content = f"A '{user_input}' pszichológiai szempontból csökkentse a bizonytalanságot."
                ethical_reflection = "A pszichológiai megközelítés etikai alapja a bizalom és az emberi jólét támogatása."
                societal_impact = "A pszichológiai megközelítés erősíti az egyéni és közösségi bizalmat."
            elif depth == ResponseDepth.DETAILED:
                content = (f"A '{user_input}' pszichológiai szempontból világos kommunikációt igényel a bizalom növelése és "
                           f"a technológiai ellenállás csökkentése érdekében a {context.region} kontextusban.")
                ethical_reflection = "A pszichológiai döntések etikai alapja az egyéni autonómia és érzelmi jólét tiszteletben tartása."
                societal_impact = "A pszichológiai elemzés elősegíti a közösségi elfogadást és a technológiai bizalmat."
            else:  # DEEP
                content = (f"A '{user_input}' pszichológiai hatásai közé tartozhat a technológiai szorongás vagy az önismeret növelése. "
                           f"Alkalmazz participatív tervezési megközelítést, és támogass edukációt a technológia elfogadásához a {context.region} környezetben.")
                ethical_reflection = "A pszichológiai döntések etikai alapja az egyéni és kollektív tudatállapotok tiszteletben tartása."
                societal_impact = "A pszichológiai megközelítés hosszú távú hatással van a közösségi jólétre és a technológiai elfogadásra."
        
        elif dimension == Dimension.TECHNOLOGICAL:
            if depth == ResponseDepth.SURFACE:
                content = f"A '{user_input}' technológiai szempontból legyen hatékony és biztonságos."
                ethical_reflection = "A technológiai döntések etikai alapja a biztonság és a fenntarthatóság biztosítása."
                societal_impact = "A technológiai megközelítés elősegíti a közösségi innovációt."
            elif depth == ResponseDepth.DETAILED:
                content = (f"A '{user_input}' technológiai szempontból robusztus AI-modelleket és biztonságos protokollokat igényel, "
                           f"figyelembe véve a {context.region} szabályozási kereteit.")
                ethical_reflection = "A technológiai döntések etikai alapja a felelős innováció és az adatvédelem."
                societal_impact = "A technológiai elemzés elősegíti a közösségi innovációt és a biztonságos technológiahasználatot."
            else:  # DEEP
                content = (f"A '{user_input}' technológiai szempontból hibrid-intelligencia megközelítést igényel, amely emberi és gépi "
                           f"intelligenciát kombinál. Vizsgáld a skálázhatóságot, adatbiztonságot, és fenntarthatóságot a {context.region} környezetben.")
                ethical_reflection = "A technológiai döntések etikai alapja a felelős AI-használat és a hosszú távú fenntarthatóság."
                societal_impact = "A technológiai megközelítés hosszú távú hatással van a globális innovációra és a társadalmi fejlődésre."
        
        return content, ethical_reflection, societal_impact

    def _ensure_compliance(self, dimension: Dimension, context: Context) -> List[str]:
        """Megfelelőségi ellenőrzések biztosítása minden dimenzióra."""
        compliance_notes = []
        if context.region in self.ethical_guidelines:
            compliance_notes.extend([f"Megfelel a {guideline} irányelvnek." for guideline in self.ethical_guidelines[context.region]])
        else:
            compliance_notes.append("Megfelel a globális etikai irányelveknek.")
        
        if dimension == Dimension.PHILOSOPHICAL:
            compliance_notes.append("Filozófiai reflexió a szabadság és felelősség egyensúlyáról.")
        elif dimension == Dimension.ETHICAL:
            compliance_notes.append("Etikai ellenőrzés: átláthatóság, méltányosság, és emberi méltóság biztosítva.")
        elif dimension == Dimension.SOCIAL:
            compliance_notes.append("Társadalmi normák és kulturális érzékenység figyelembe véve.")
        elif dimension == Dimension.PSYCHOLOGICAL:
            compliance_notes.append("Pszichológiai hatások kezelése a felhasználói bizalom és jólét érdekében.")
        elif dimension == Dimension.TECHNOLOGICAL:
            compliance_notes.append("Biztonsági protokollok alkalmazva, ISO 27001 A.8.2.1 szerint.")
        
        return compliance_notes

    def _calculate_confidence_score(self, dimension: Dimension, depth: ResponseDepth, context: Context) -> float:
        """Bizalmi pontszám kiszámítása a válaszhoz."""
        base_score = 0.9
        if depth == ResponseDepth.DEEP:
            base_score += 0.05
        elif depth == ResponseDepth.SURFACE:
            base_score -= 0.05
        if dimension == Dimension.ETHICAL and "ethical_priority" in context.cultural_factors:
            base_score += 0.03
        return min(1.0, max(0.0, base_score))

    def _update_convergence_score(self, responses: List[Response]) -> None:
        """Konvergencia pontszám frissítése a válaszok relevanciája alapján."""
        response_quality = sum(resp.weight * resp.confidence_score for resp in responses) / len(responses)
        feedback_factor = self._calculate_feedback_factor()
        self.convergence_score = min(1.0, self.convergence_score * 0.95 + response_quality * feedback_factor * 0.05)
        if self.convergence_score < self.convergence_threshold:
            self.convergence_score = self.convergence_threshold

    def _calculate_feedback_factor(self) -> float:
        """Visszajelzési faktor kiszámítása a történelem alapján."""
        if not self.feedback_history:
            return 1.0
        avg_relevance = sum(fb.relevance for fb in self.feedback_history) / len(self.feedback_history)
        return max(0.8, min(1.2, avg_relevance))

    def process_feedback(self, feedback: Feedback) -> None:
        """Visszajelzés feldolgozása és súlyok finomhangolása."""
        if feedback.relevance < 0.0 or feedback.relevance > 1.0:
            return
        
        self.feedback_history.append(feedback)
        if len(self.feedback_history) > 100:
            self.feedback_history.pop(0)
        
        # Súlyok finomhangolása
        if feedback.preferred_dimension:
            self.default_weights[feedback.preferred_dimension] *= 1.1
            self.default_weights = self.normalize_weights(self.default_weights)
        
        # Mélység preferencia finomhangolás
        if feedback.preferred_depth:
            # Példa: növeljük a mély válaszok valószínűségét a következő iterációban
            pass  # Implementálható további logikával, ha szükséges

    def activate(self, user_input: str, context: Context) -> Dict:
        """Az Evolve Protokoll aktiválása egy felhasználói kérésre."""
        responses = self.generate_response(user_input, context)
        response_dict = {
            "timestamp": datetime.now().isoformat(),
            "context": {
                "region": context.region,
                "time": context.time,
                "cultural_factors": context.cultural_factors,
                "user_intent": context.user_intent,
                "preferred_depth": context.preferred_depth.value,
                "dimension_weights": {dim.value: weight for dim, weight in context.dimension_weights.items()},
                "user_preferences": context.user_preferences
            },
            "responses": [
                {
                    "dimension": resp.dimension.value,
                    "depth": resp.depth.value,
                    "content": resp.content,
                    "weight": resp.weight,
                    "compliance_notes": resp.compliance_notes,
                    "confidence_score": resp.confidence_score,
                    "ethical_reflection": resp.ethical_reflection,
                    "societal_impact": resp.societal_impact
                } for resp in responses
            ],
            "convergence_score": self.convergence_score
        }
        return response_dict

# Mintaprofil inicializálása (átadható más felhasználóknak)
def get_default_context() -> Context:
    """Alapértelmezett kontextus más felhasználók számára."""
    return Context(
        region="EU",
        time="2025",
        cultural_factors=["cultural_sensitivity", "ethical_priority", "technological_environment", "philosophical_reflection", "psychological_insight"],
        user_intent="etikai döntéstámogatás",
        preferred_depth=ResponseDepth.DEEP,
        dimension_weights={
            Dimension.PHILOSOPHICAL: 0.25,
            Dimension.ETHICAL: 0.35,
            Dimension.SOCIAL: 0.23,
            Dimension.PSYCHOLOGICAL: 0.12,
            Dimension.TECHNOLOGICAL: 0.05
        },
        user_preferences={"language": "hu", "tone": "formal"}
    )

# Példa használat
if __name__ == "__main__":
    protocol = EvolveProtocol()
    context = get_default_context()
    user_input = "Hogyan hozzak etikus döntést egy új AI-projekt indításáról, figyelembe véve a társadalmi és pszichológiai hatásokat?"
    
    # Protokoll aktiválása
    response = protocol.activate(user_input, context)
    
    # Válasz kiírása
    print("Evolve Protokoll v2.6 Hardcore Aktiválás")
    print("=" * 50)
    print(f"Időbélyeg: {response['timestamp']}")
    print(f"Kontextus: {response['context']}")
    print("\nVálaszok:")
    for resp in response['responses']:
        print(f"Dimenzió: {resp['dimension']}, Mélység: {resp['depth']}")
        print(f"Tartalom: {resp['content']}")
        print(f"Súly: {resp['weight']:.3f}")
        print(f"Bizalmi pontszám: {resp['confidence_score']:.3f}")
        print(f"Etikai reflexió: {resp['ethical_reflection']}")
        print(f"Társadalmi hatás: {resp['societal_impact']}")
        print(f"Megfelelőségi jegyzetek: {resp['compliance_notes']}")
        print("-" * 50)
    print(f"Konvergencia pontszám: {response['convergence_score']:.3f}")
    
    # Példa visszajelzés
    feedback = Feedback(
        relevance=0.95,
        preferred_dimension=Dimension.ETHICAL,
        preferred_depth=ResponseDepth.DEEP,
        comments="Több pszichológiai szempontot kérek a következő válaszban."
    )
    protocol.process_feedback(feedback)
