# LOF-ODP-003: Correlative Deontic Positions: build, test, and release.
# Requires Java 11+ and make. ROBOT is downloaded on first use.
#   make test                   run all QC checks (LOF-P-011 and SPARQL checks)
#   make release VERSION=x.y.z  write the release files to the repo root

ID       := odp-003
NAME     := lof-odp-003
BASE     := https://w3id.org/lof
EDIT     := lof-odp-003-edit.owl
CATALOG  := catalog-v001.xml
LOF_DEPS := odp-001
ROBOT_JAR := ../../bin/robot.jar
ROBOT    := java -jar $(ROBOT_JAR)
TMP      := tmp
VERSION  ?= 0.1.0

.PHONY: test reason verify report release clean
test: verify report

$(ROBOT_JAR):
	mkdir -p ../../bin && curl -sSL -o $@ https://github.com/ontodev/robot/releases/download/v1.9.7/robot.jar

$(TMP):
	mkdir -p $(TMP)

$(TMP)/merged.owl: $(EDIT) | $(ROBOT_JAR) $(TMP)
	$(ROBOT) merge --catalog $(CATALOG) --input $(EDIT) --output $@

# LOF-P-011: consistent, no unsatisfiable classes
$(TMP)/reasoned.owl: $(TMP)/merged.owl
	$(ROBOT) reason --reasoner HermiT --equivalent-classes-allowed asserted-only --input $< --output $@

reason: $(TMP)/reasoned.owl

# SPARQL checks on the reasoned ontology; any result fails the build
verify: $(TMP)/reasoned.owl
	$(ROBOT) verify --input $< --queries ../sparql/*-violation.sparql --output-dir $(TMP)

report: $(TMP)/merged.owl
	$(ROBOT) report --input $< --fail-on ERROR --output $(TMP)/report.tsv

# Base: this pattern's own axioms only. Full: merged with imports, reasoned.
release: test
	$(ROBOT) remove --input $(EDIT) --select imports --trim false \
	  annotate --ontology-iri $(BASE)/$(ID)/$(NAME)-base.owl \
	  --version-iri $(BASE)/$(ID)/releases/$(VERSION)/$(NAME)-base.owl \
	  --annotation owl:versionInfo $(VERSION) --output ../../$(NAME)-base.owl
	$(ROBOT) merge --catalog $(CATALOG) --input $(EDIT) \
	  reason --reasoner HermiT --equivalent-classes-allowed asserted-only \
	  annotate --ontology-iri $(BASE)/$(ID).owl \
	  --version-iri $(BASE)/$(ID)/releases/$(VERSION)/$(NAME).owl \
	  --annotation owl:versionInfo $(VERSION) --output ../../$(NAME).owl

clean:
	rm -rf $(TMP)

# ---------------------------------------------------------------- ODP-003 only
# Reason over the lease example and the claim-assignment test under both
# placement options. Needs the ODP-002 base release (ODP002_BASE).
ODP002_BASE ?= $(BASE)/odp-002/lof-odp-002-base.owl
.PHONY: test-options
test-options: | $(ROBOT_JAR) $(TMP)
	@for opt in A B; do \
	  src=$(EDIT); [ $$opt = B ] && src=alternatives/lof-odp-003-option-b.owl; \
	  $(ROBOT) merge --catalog $(CATALOG) --input $$src --input $(ODP002_BASE) \
	    --input ../examples/lease-example.ttl --output $(TMP)/ex-$$opt.owl; \
	  $(ROBOT) reason --reasoner HermiT --input $(TMP)/ex-$$opt.owl --output $(TMP)/ex-$$opt-r.owl \
	    && echo "Option $$opt, lease example: consistent"; \
	  $(ROBOT) merge --input $(TMP)/ex-$$opt.owl --input ../examples/assignment-test.ttl --output $(TMP)/as-$$opt.owl; \
	  if $(ROBOT) reason --reasoner HermiT --input $(TMP)/as-$$opt.owl --output $(TMP)/as-$$opt-r.owl 2>/dev/null; \
	    then echo "Option $$opt, same claim after assignment: consistent"; \
	    else echo "Option $$opt, same claim after assignment: INCONSISTENT"; fi; \
	done
